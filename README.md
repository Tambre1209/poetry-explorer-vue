***State Management***
State is managed natively through a centralized object in App.vue and passed down via prop drilling. Because the applciation component tree maxes out at three levels deep, this avoids needing a separate dedicated state library like Pinia. This can be refactored into a composable later if desired.
