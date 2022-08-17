# RW_iOS_Bootcamp_Fall_2022: Questions

**Question on use of .transition()**

I'm looking for a clarification, please, on video **“28. Intro to SwiftUI Animation”** in the second course on Polishing the App-

Around timestamp 6:45, Ray is adding the ```.transition(.scale)``` modifier to several views in the app:

He adds it to the ```PointsView``` struct in ```PointsView.swift```

```swift
struct PointsView: View {

    @Binding var alertIsVisible: Bool
    @Binding var sliderValue: Double
    @Binding var game: Game

    var body: some View {

        let roundedValue = Int(sliderValue.rounded())
        let points = game.points(sliderValue: roundedValue)

        VStack(spacing: 10){
            InstructionText(text: "The slider's value is")
            BigNumberText(text:String(roundedValue))
            BodyText(text:"You scored \(points) Points\n🎉🎉🎉")
            Button(action: {
                withAnimation {
                    alertIsVisible = false
                }
                game.startNewRound(points: points)
            }) {
                ButtonText(text: "Start New Round")
            }

        }
            .padding()
            .frame(maxWidth: 300)
            .background(
                Color("BackgroundColor")
            )
            .cornerRadius(21.0)
            .shadow(radius: 10, x: 5, y:5)
            .transition(.scale)
       
    }
}
```

He also adds it to three places in ```ContentView.swift```:

To ```SliderView```, to ```HitMeButton``` and to ```PointsView``` in the body:

```swift
struct ContentView: View {

    @State private var alertIsVisible = false
    @State private var sliderValue = 50.0
    @State private var game = Game()

    var body: some View {

        ZStack {
            BackgroundView(game: $game)
            VStack {
                InstructionsView(game: $game)
                    .padding(.bottom, alertIsVisible ? 0 : 100)

             // PointsView(), HitMeButton() and SliderView()
             // Are coming in and out, so add transition

                if alertIsVisible {
                    PointsView(alertIsVisible: $alertIsVisible, sliderValue: $sliderValue, game: $game)
                        .transition(.scale)
                } else {

                    HitMeButton(alertIsVisible: $alertIsVisible, sliderValue: $sliderValue, game: $game)
                        .transition(.scale)
                }
            }
            if !alertIsVisible {
                SliderView(sliderValue: $sliderValue)
                    .transition(.scale)
            }
        }
    }
}
```

Why is  ```.transition(.scale)``` added to ```PointsView``` in ```PointsView.swift``` and not to the structs where ```SliderView``` and ```HitMeButton``` are defined in ```ContentView.swift```?

I played around with it a bit and these were my observations:

  * Removing the line from the ```PointsView``` struct in ```PointsView.swift``` seemed to have no impact on how the app behaved; all three views scaled up and down as they came and went, as expected.
 
  * Still with the line removed from ```PointsView.swift```, I changed the line of code on ```PointsView``` in ```ContentView``` to ```.transition(.slide)``` to more clearly see the animation. The custom alert slid in from the left when it appeared and slid out toward the right when it disappeared, so the app again behaved as expected with the line of code still absent from ```PointsView.swift```.

  * When I put ```.transition(.slide)``` in ```PointsView.swift``` and ```.transition(.scale)``` on ```PointsView``` in ```ContentView.swift```, ```.transition(.slide)``` superseded ```transition(.scale)```. My thinking is you wouldn't want to add a transition to ```PointsView.swift``` directly because you may want different behavior in ```ContentView.swift``` and it might be hard to run down why you're seeing something different than what you're expecting.  Thoughts on this point?  Thanks for reading!
