# PreCalc-Bresenhams
I call it PreCalc'd to Hell Bresenhams, or PCHB. The entire precalc factorization of the Bresenham's line algorithm... unfortunately slower.

```C++
void PCHB(int x1, int y1, int x2, int y2) {
    int dx = x2 - x1, dy = y2 - y1,
        // dxyA is the sign of the quadrant xy delta.
        dxA = sgn(dx), dyA = sgn(dy),
        // dyB is the absolute quadrant xy delta (to isolate the quadrant math).
        dxB = abs(dx), dyB = abs(dy),
        // check if x>y or y>x for quadrant determination.
        cx = dxB >= dyB, cy = dyB >= dxB,
        // qx is whether we're in a horz-x facing quadrant.
        // qy is whether we're in a vert-y facing quadrant.
        qx = cy * dxB, qy = cx * dyB,
        // qr checks if we lie in a quadrant rather than one of the 8 cardinal dir.
        // pd is for the incremental error check below.
        qr = qx != qy, pd = qx + qy,
        // if the line is horz, move horz other move vert.
        xm = cx * dxA, ym = cy * dyA,
        // if the line is horz, move horz other move vert.
        xym = cx? dxB : dyB,
        // Incremental error check (see Bresenhams algorithm).
        er = pd - (xym/2), ec;

    // Create a lookup table, rather than use multiplication in the for(;;) below.
    // look*[0] is if the line is horz, vert or diag.
    // look*[1] is if the line is in between angles (direction is not mod 45 == 0).
    int lookx[2] = {xm,xm + (qr * cy * dxA)},
        looky[2] = {ym,ym + (qr * cx * dyA)},
        lookd[2] = {qr * pd, qr * (pd - xym)};

    //draw_point(xx, yy);
    for(;;) {
        // Error check above/below the line.
        ec = er >= 0;
        // Increment lookup table based on error check.
        // ec==0 -> line is horz/vert/diagonal (dir%45 = 0).
        // ec==1 -> line is between cardinals (dir%45 != 0).
        x1 += lookx[ec];
        y1 += looky[ec];
        er += lookd[ec];
        // Break loop when line is done.
        //draw_point(xx, yy);
        if (x2 == x1 && y2 == y1) break;
    };
};
```
