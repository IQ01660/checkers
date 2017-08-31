# checkers
```C#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace checkers
{
    public partial class Form1 : Form
    {
        bool firstChoose = false;
        bool redTurn = true;
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            // creating the playground
            int leftPosField = 40;
            int topPosField = 20;

            for (int rows = 0; rows < 8; rows++)
            {
                for (int cols = 0; cols < 8; cols++)
                {
                    FieldHolder.fieldMatrix[rows, cols] = new Button();
                    FieldHolder.fieldMatrix[rows, cols].Size = new System.Drawing.Size(70, 70);
                    FieldHolder.fieldMatrix[rows, cols].Text = "";
                    FieldHolder.fieldMatrix[rows, cols].Left = leftPosField;
                    FieldHolder.fieldMatrix[rows, cols].Top = topPosField;
                    // deleting borders
                    FieldHolder.fieldMatrix[rows, cols].TabStop = false;
                    FieldHolder.fieldMatrix[rows, cols].FlatStyle = FlatStyle.Flat;
                    FieldHolder.fieldMatrix[rows, cols].FlatAppearance.BorderSize = 0;
                    FieldHolder.fieldMatrix[rows, cols].FlatAppearance.BorderColor = Color.FromArgb(0, 255, 255, 255); //transparent
                    // even && even || odd && odd
                    if ((rows % 2 == 0 && cols % 2 == 0) || (rows % 2 == 1 && cols % 2 == 1))
                    {
                        FieldHolder.fieldMatrix[rows, cols].BackColor = Color.LightYellow;

                        Controls.Add(FieldHolder.fieldMatrix[rows, cols]);
                    }
                    // even && odd || odd && even
                    else
                    {
                        FieldHolder.fieldMatrix[rows, cols].BackColor = Color.Black;

                        Controls.Add(FieldHolder.fieldMatrix[rows, cols]);

                        if (rows == 0 || rows == 1 || rows == 2)
                        {

                            FieldHolder.fieldMatrix[rows, cols].Text = "o";
                            FieldHolder.fieldMatrix[rows, cols].ForeColor = Color.Red;
                            FieldHolder.fieldMatrix[rows, cols].Font = new System.Drawing.Font("Microsoft Sans Serif", 32F, System.Drawing.FontStyle.Bold, System.Drawing.GraphicsUnit.Point, ((byte)(204)));
                            EmptyInfoHolder.emptyData[rows, cols] = false;

                        }
                        if (rows == 5 || rows == 6 || rows == 7)
                        {
                            FieldHolder.fieldMatrix[rows, cols].Text = "o";
                            FieldHolder.fieldMatrix[rows, cols].ForeColor = Color.White;
                            FieldHolder.fieldMatrix[rows, cols].Font = new System.Drawing.Font("Microsoft Sans Serif", 32F, System.Drawing.FontStyle.Bold, System.Drawing.GraphicsUnit.Point, ((byte)(204)));
                            EmptyInfoHolder.emptyData[rows, cols] = false;
                            FieldHolder.fieldMatrix[rows, cols].Click += new System.EventHandler(this.white_Click);
                        }
                        if (EmptyInfoHolder.emptyData[rows, cols] == true)
                        {
                            FieldHolder.fieldMatrix[rows, cols].Click += new System.EventHandler(this.space_Click);
                        }
                    }
                    leftPosField += 70;
                }
                leftPosField = 40;
                topPosField += 70;
            }

        }

        private void white_Click(object sender, EventArgs e)
        {
            var myFigure = sender as Button;
            foreach(var item in FieldHolder.fieldMatrix)
            {
                if (item.BackColor == Color.DarkBlue)
                {
                    item.BackColor = Color.Black;
                }
            }
            FigureHolder.figure.Clear();
            FigureHolder.figure.Add(myFigure);
            FigureHolder.figure[0].BackColor = Color.DarkBlue; 
            if (firstChoose == false)
            {
                firstChoose = true;
            }
        }
        private void space_Click(object obj, EventArgs eve)
        {
            var mySpace = obj as Button;
            if (firstChoose == true && mySpace.Text != "o" && FigureHolder.figure[0].Top - mySpace.Top == 70 && (Math.Abs(mySpace.Left - FigureHolder.figure[0].Left) == 70))
            {
                
                mySpace.Text = "o";
                mySpace.ForeColor = Color.White;
                mySpace.Font = new System.Drawing.Font("Microsoft Sans Serif", 32F, System.Drawing.FontStyle.Bold, System.Drawing.GraphicsUnit.Point, ((byte)(204)));
                mySpace.Click += new System.EventHandler(this.white_Click);
                mySpace.Click -= new System.EventHandler(this.space_Click);
                FigureHolder.figure[0].Text = "";
                FigureHolder.figure[0].BackColor = Color.Black;
                FigureHolder.figure[0].Click -= new System.EventHandler(this.white_Click);
                FigureHolder.figure[0].Click += new System.EventHandler(this.space_Click);
                firstChoose = false;
                // moving red figures
                if (redTurn == true)
                {
                    FieldHolder.fieldMatrix[2, 5].Text = "";
                    FieldHolder.fieldMatrix[3, 6].Text = "o";
                    FieldHolder.fieldMatrix[3, 6].Font = new System.Drawing.Font("Microsoft Sans Serif", 32F, System.Drawing.FontStyle.Bold, System.Drawing.GraphicsUnit.Point, ((byte)(204)));
                    FieldHolder.fieldMatrix[3, 6].ForeColor = Color.Red;

                    FieldHolder.fieldMatrix[3, 6].Click -= new System.EventHandler(this.space_Click);

                    FieldHolder.fieldMatrix[2, 5].Click += new System.EventHandler(this.space_Click);
                    redTurn = false;
                }
            }
        }
    }
    class FieldHolder
    {
        static public Button[,] fieldMatrix = new Button[8, 8];
    }
    class EmptyInfoHolder
    {
        static public bool[,] emptyData = new bool[8, 8] { { true, true, true, true, true, true, true, true }, { true, true, true, true, true, true, true, true }, { true, true, true, true, true, true, true, true }, { true, true, true, true, true, true, true, true }, { true, true, true, true, true, true, true, true }, { true, true, true, true, true, true, true, true }, { true, true, true, true, true, true, true, true }, { true, true, true, true, true, true, true, true } };
    }
    class FigureHolder
    {
        static public List<Button> figure = new List<Button>();
    }
    class FigureLeftHolder
    {
        static public List<int> figureLeft = new List<int>();
    }
    class FigureTopHolder
    {
        static public List<int> figureTop = new List<int>();
    }

}
```
