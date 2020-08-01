# PreCalc-Bresenhams
I call it PreCalc'd to Hell Bresenhams, or PCHB. The entire precalc factorization of the Bresenham's line algorithm... This one is hard to explain... Lots of refactoring and pre-calc'ing.
```C++
void pchb(int x1, int y1, int x2, int y2) {
    int dx = x2 - x1, dy = y2 - y1,
        dxA = sign(dx), dyA = sign(dy),
        dxB = abs(dx), dyB = abs(dy),
        xm=0, ym=dyA,
        yxm=dxB, xym=dyB;
    
    if (dxB >= dyB) {
        xm = dxA; ym = 0;
        yxm = dyB; xym = dxB;
    }

    int er2 = yxm - xym,
        er = yxm - (xym>>1);
    
    // draw starting pixel (x1,y1).
    for(;x1!=x2 || y1!=y2;) {
        if (er >= 0) {
            x1 += dxA;
            y1 += dyA;
            er += er2;
        } else {
            x1 += xm;
            y1 += ym;
            er += yxm;
        }
        
        // draw current pixel (x1,y1).
    }
};
```
Just FYI I used this nifty type safe sign function:
```C++
template <typename T> int sign(T val) {
    return (T(0) < val) - (val < T(0));
}
```
