# LoadingAnimation
Loading Animation with Jetpack Compose | Android Studio Tutorial





@Composable
fun LoadingAnimation(
    modifier: Modifier = Modifier,
    circleSize: Dp = 25.dp,
    circleColor: Color = Blue,
    spaceBetween: Dp = 10.dp,
    travelDistance: Dp = 20.dp
) {
    val circles = listOf(
        remember { Animatable(initialValue = 0f) },
        remember { Animatable(initialValue = 0f) },
        remember { Animatable(initialValue = 0f) }
    )
    val circleValues = circles.map { it.value }
    val distance = with(LocalDensity.current) { travelDistance.toPx() }
    val lastCircle = circleValues.size - 1
    circles.forEachIndexed { index, animatable ->
        LaunchedEffect(key1 = animatable) {
            delay(index * 200L)
            animatable.animateTo(
                targetValue = 1f,
                animationSpec = infiniteRepeatable(
                    animation = keyframes {
                        durationMillis = 1500
                        0.0f at 0 with LinearOutSlowInEasing
                        1.0f at 500 with LinearOutSlowInEasing
                        0.0f at 1000 with LinearOutSlowInEasing
                        0.0f at 1500 with LinearOutSlowInEasing
                    },
                    repeatMode = RepeatMode.Restart
                )
            )
        }
    }



    Row(modifier = modifier) {
        circleValues.forEachIndexed { index, value ->
            Box(modifier = Modifier
                .size(circleSize)
                .graphicsLayer {
                    translationY = -value * distance
                }
                .background(circleColor, shape = CircleShape))
            if (index != lastCircle) {
                Spacer(modifier = Modifier.width(spaceBetween))
            }
        }
    }
}

@Preview(showBackground = true)
@Composable
fun LoadingAnimationPreview() {
    LoadingAnimation()
}
