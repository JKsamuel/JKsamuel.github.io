---
title: To make a simple graphical interface
tags: 
 - Graphics object
 - Timer control
 - TrackBar control
 - C#
 - .NET
description: Using Graphics objectand Timer control
---
Target framework 4.7.2

# To make a simple graphical interface


This was my one of assignment of C# course.

I guess the purpose of this assignment seems to be check whether I know how to use timer control.

The output of this assignment is below   
![assignment5](/assets/img/example.gif)

You may not believe it, but the figure above is a bucket of water. The speed at which water is filled in the bucket is expressed differently using the TrackBar control.

```c#
public partial class FrmCanvas : Form
    {
        private Graphics graphics;
        private Pen pen;
        private SolidBrush brush;
        private Color c = Color.White;
        private int fillup;
        private int fallingdown;
        private bool isSleep = true;

        public FrmCanvas()
        {
            InitializeComponent();
            this.Paint += new PaintEventHandler(Frm_Canvas);
            
        }

        private void Frm_Canvas(object sender, PaintEventArgs e)
        {
            graphics = e.Graphics;
            brush = new SolidBrush(c);
            pen = new Pen(c);
            graphics = this.CreateGraphics();
            graphics.DrawLine(pen, 100, 300, 100, 550);
            graphics.DrawLine(pen, 400, 300, 400, 550);
            
        }

        private void btnColour_Click(object sender, EventArgs e)
        {
            colorSeletor.Color = c;
            colorSeletor.ShowDialog();
            c = colorSeletor.Color;
        }

        private void btnExit_Click(object sender, EventArgs e)
        {
            Application.Exit();
        }

        private void trackBar_Scroll(object sender, EventArgs e)
        {
            timer_thread.Stop();
            lblGauge.Text = trackBar.Value.ToString();
            int value = trackBar.Value;
            timer_thread.Interval = 100 / value;
            timer_thread.Enabled = true;
            
        }

        private void timer_thread_Tick(object sender, EventArgs e)
        {
            int x = 100;
            int y = 530;
            int width = 300;
            double height = 1.5;
            int amount = 230;

            if(timer_thread.Enabled && fillup != amount)
            {
                Rectangle water = new Rectangle(125, 152 + fallingdown, 8, 400);
                graphics.FillRectangle(brush, water);

                fillup++;
                Rectangle rects = new Rectangle(x, y - fillup, width, (int)height);

                graphics.FillRectangle(brush, rects);

                if (fillup == amount)
                {
                    timer_thread.Enabled = false;
                    Rectangle screen = new Rectangle(125, 152, 8, 148);
                    SolidBrush black = new SolidBrush(Color.Black);
                    graphics.FillRectangle(black, screen);

                }
            }
            
        }
    }
```

I haven't received feedback on this assignment yet, so I'm not sure how well I did. In fact, I have yet to submit it.   

If I think of the **Timer control** as a **loop statement**, I believe it can be easily used. System contains the Timer class, which is useful for Winform applications.   

I achieved the above results by utilising the Timer class's **Tic event** and **Interval method**.

When the Interval method is used to set the interval time and the Timer is enabled, an event occurs after the set time. I used the For loop statement in the Timer Tic event method without realising it. The intended result was for the speed at which the water was filled to vary depending on the value set for TrackBar, but when I wrote the code as shown above, the water was completely filled after the set interval time.

Thus, in the Tic event method, a flag variable called fillup was added and increased one by one each time it was repeated, and Timer was deactivated when the fillup variable reached the set value.