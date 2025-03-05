
## 📚 Delphi Direct2D Tips:

Welcome to **Delphi Direct2D Tips**, a plain-text guide with essential tips for building and improving Direct2D apps using Delphi. This guide includes tips based on our previous Expirement to help you solve specific issues.

---

<details>
<summary>Tip1: Direct2D Effects in Delphi (After using our new unit bellow) 🌟</summary>


 **Problem:**  
the Delphi built-in units D2D are missing lot of interfaces including "ID2D1Effect".

 **Overview:**  

This guide explains how to apply Direct2D effects in Delphi using a clean and reusable iD2DEffect interface. The provided API.D2D.Effects unit encapsulates Direct2D effects, allowing you to:

  Apply Gaussian blur, shadow, and other effects.
  Set input images for the effect.
  Chain method calls for cleaner code.
  Use multiple property types (Single, Integer, Boolean).
  Safely render effects without memory leaks.

![MS Doc](https://learn.microsoft.com/en-us/windows/win32/direct2d/effects-overview)  

 **Solution:**  
 you can use our under-dev unit in your own-risk!
 ----  
Creating and Applying Direct2D Effects

Step 1: Initialize the Effect
```pascal
uses
  ...
  API.D2D.Effects;

type
  TMainView = class(TForm)
  strict private
    fD2DCanvas: TDirect2DCanvas;
    fD2DEffect: iD2DEffect;
    function GetD2DEffect: iD2DEffect;
...
  public
    constructor Create(AOwner: TComponent); override;
    destructor Destroy; override;

    property D2DEffect: iD2DEffect read GetD2DEffect;
  end;

{ TMainView }

function TMainView.GetD2DEffect: iD2DEffect;
begin
  if not Assigned(fD2DEffect) then
    fD2DEffect := GetTDirect2DEffect(fD2DCanvas.RenderTarget, CLSID_D2D1GaussianBlur);

  Result := fD2DEffect;
end;
```
  
Step 2: Set Input and Configure Effect 
```pascal
      D2DEffect.SetInputBitmap(0, LD2DBitmap)

      .(0, D2D1_GAUSSIANBLUR_PROP_STANDARD_DEVIATION, 3.5)

      .(0, D2D1_GAUSSIANBLUR_PROP_OPTIMIZATION,
           D2D1_DIRECTIONALBLUR_OPTIMIZATION_SPEED)

      .(0, D2D1_GAUSSIANBLUR_PROP_BORDER_MODE,
           D2D1_BORDER_MODE_HARD);

          // Begin Direct2D drawing
      fD2DCanvas.BeginDraw;
      try
        // Draw blured image
        D2DEffect.DrawEffectImage;

        // Optionally, draw additional UI elements on top
        // fD2DCanvas.DrawRectangle(...);
      finally
        fD2DCanvas.EndDraw;
      end;
...etc
```
Step 3: Render the Effect  
```pascal
D2DEffect.DrawEffectImage;
```
Features & Benefits

✅ Encapsulated Direct2D Effect Handling

Instead of manually managing effect creation, this unit provides a structured interface-based approach.

✅ Method Chaining

Write cleaner and more readable effect configurations using method chaining.

✅ Multiple Data Type Support

The ApplyEffect method supports different types:
```pascal
D2DEffect.ApplyEffect(aIndex, aPropType, 5.0);  // Single (Float)
D2DEffect.ApplyEffect(aIndex, aPropType, 1);    // Integer
D2DEffect.ApplyEffect(aIndex, aPropType, True); // Boolean
```
✅ Automatic Memory Management

Using interfaces (iD2DEffect), the effect is automatically freed when no longer needed, preventing memory leaks.  
## Full Example:
```pascal
var
  LD2DEffect: iD2DEffect;
begin
  LD2DEffect := GetTDirect2DEffect(SomeRenderTarget, CLSID_D2D1GaussianBlur);
  
  LD2DEffect.SetInputBitmap(0, SomeD2DBitmap)
      .(0, D2D1_GAUSSIANBLUR_PROP_STANDARD_DEVIATION, 3.5)
      .(0, D2D1_GAUSSIANBLUR_PROP_OPTIMIZATION,
           D2D1_DIRECTIONALBLUR_OPTIMIZATION_SPEED)
      .(0, D2D1_GAUSSIANBLUR_PROP_BORDER_MODE,
           D2D1_BORDER_MODE_HARD);
end;
```
How It Works Internally

GetTDirect2DEffect: Creates a new effect instance.

SetInputBitmap: Assigns an input bitmap to the effect.

ApplyEffect: Sets effect properties dynamically.

DrawEffectImage: Draws the effect output onto the render target.  

## Closing Note:  

We hope this tip helps you improve your Delphi Direct2D app development experience. Stay tuned for more tips and Updates!

Happy coding! 🚀



</details>
