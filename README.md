package com.example.waveanimation

import android.animation.ValueAnimator
import android.content.Context
import android.graphics.Canvas
import android.graphics.Paint
import android.graphics.Path
import android.util.AttributeSet
import android.view.View
import androidx.core.content.ContextCompat

class WaveView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : View(context, attrs, defStyleAttr) {

    private val paint = Paint().apply {
        color = ContextCompat.getColor(context, R.color.waveColor)
        style = Paint.Style.STROKE
        strokeWidth = 5f
    }

    private val path = Path()
    private var waveHeight = 50f
    private var waveLength = 50f
    private var waveOffset = 0f

    init {
        setupAnimation()
    }

    override fun onDraw(canvas: Canvas) {
        super.onDraw(canvas)
        path.reset()

        val halfWaveHeight = waveHeight / 2

        path.moveTo(width.toFloat() / 2, -waveLength + waveOffset)
        var y = -waveLength + waveOffset

        while (y < height + waveLength) {
            path.rQuadTo(-waveHeight, halfWaveHeight / 2, 0f, halfWaveHeight)
            path.rQuadTo(waveHeight, halfWaveHeight / 2, 0f, halfWaveHeight)
            y += waveLength
        }

        canvas.drawPath(path, paint)
    }

    private fun setupAnimation() {
        val animator = ValueAnimator.ofFloat(0f, waveLength)
        animator.duration = 500
        animator.repeatCount = ValueAnimator.INFINITE
        animator.addUpdateListener { animation ->
            waveOffset = animation.animatedValue as Float
            invalidate()
        }
        animator.start()
    }
}
