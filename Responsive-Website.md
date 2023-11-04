# Essential to know to create Responsive behaviour of website

# Units:
- px
- %
    - parent ke kitne % use krna hai ?, $ kiske perspective se le rhe ho, it depends on the parent.
    - parent ke update depend krta hai
- vw, vh (screen ke prespective se height and width de rhe ho)
    - view port width 
    - view port height
-vmax, vmin 
    - view port max - height jayeda hogi width se to ye vh ki properties lega srf
    - view port min
- em , rem
 - 1 em: jistna parent ko font size dia hai, uska 2 guna
 - 1 rem: puri screen ke font size ke according lega current text ya div ka size

# Layout of Website

absolute: window ke sath potition lega, mtlb top:0, left:0, window ki location se count hoga
relative: div ki posisiton lega, mtlb relative mae jha exists krta hai element vhi se top, left 0 hoga

agar parent relative and child absolute, then child parent ki left:0 , top:0 se count krega, or parent jha pr exists krta hai vhi rhega

- km se km position relative ka use krna hai, display flex ka use kro
- axis:
    - (main- horizontal, cross- vertical)
- align-items: center; cross axis mae align kr dega
- justify-contnet: center; main axis mae center mae align krega
- apne size ko shirnk kr lete hai when screen size small
- parent ko ek property dete ho: flex-wrap:wrap (element ko shrink na kre wrap kre, ek point aayega jb dbne ki stithi mae aayenge to wrap kr denge) - kum jagah mae ikhata krna
- rows form ko col form mae krne ke liye: flex-direction:column
- display flex ka use jayeda krna hai hme

# CSS Media Queries
screen ke size ke hisab se content ko change kre

## min height, min width

```css
@media (max-width: 600px){
    // iske andar jo bhi likhenge jb meri width jayeda se jayeda 600 px hai
    // jayedatar phones hote hai 600 px ke andar aa jati hai, sare portrait device aa jate hai, standard size
    // css likho yha
}
```

## max height, max width

# Key points to remember:
- css flex box
- css units
- responsive typography honi chahiye
- Mobile First Apporach
- Flexible Images and Media

# calc() function
```css
.content {
    height: calc(100% - 100px) // space after and before sub sign required.
}
```
