<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp1"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <TabControl 
            SelectedIndex="2"
            TabStripPlacement="Bottom"
            BorderThickness="2"
            BorderBrush="Beige"
            >
            <TabItem>
                <TabItem.Header>
                    <StackPanel Orientation="Horizontal">
                        <Label Content ="ContentControl"/>
                        <Rectangle Width="10"
                                   Height="10"
                                   Fill="Green"/>
                    </StackPanel>

                </TabItem.Header>
                <StackPanel>
                    <ItemsControl>
                        <Label>jakis label</Label>
                        <Label>jakis label</Label>
                        <Label>jakis label</Label>
                    </ItemsControl>
                    <Separator/>
                    <ItemsControl ItemsSource="{Binding ListaSlow}">

                    </ItemsControl>
                    <Separator/>
                    <ItemsControl x:Name="lista3"/>
                    <Separator/>
                    <ItemsControl ItemsSource="{Binding Produkty}">
                        <ItemsControl.ItemTemplate>
                            <DataTemplate>
                                <StackPanel Orientation="Horizontal">
                                    <Label Content="{Binding Nazwa}"/>
                                    <Label Content="{Binding Opis}"/>
                                </StackPanel>
                            </DataTemplate>
                        </ItemsControl.ItemTemplate>
                    </ItemsControl>
                </StackPanel>
            </TabItem>
            <TabItem Header="ListBox">
                <ListBox ItemsSource="{Binding Produkty}"
                         SelectedItem="{Binding ZaznaczonyElement}">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <StackPanel Orientation="Horizontal">
                                <Label Content="{Binding Nazwa}"/>
                                <Label Content="{Binding Opis}"/>
                                <TextBlock Text="{Binding Cena}"/>
                            </StackPanel>
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </TabItem>
            <TabItem Header="listView">
                <ListView ItemsSource="{Binding Produkty}"
                          SelectedItem="{Binding ZaznaczonyElement}"
                          SelectionMode="Multiple">
                    <ListView.View>
                        <GridView>
                            <GridViewColumn
                                DisplayMemberBinding="{Binding Nazwa}"
                                Header="Nazwa produktu"/>
                            <GridViewColumn 
                                DisplayMemberBinding="{Binding Cena}"
                                Header="Cena"/>
                            <GridViewColumn
                                DisplayMemberBinding="{Binding Opis}"
                                Header="Opis"/>
                        </GridView>
                    </ListView.View>
                </ListView>
            </TabItem>
            <TabItem Header="dataGrid">
                <StackPanel>
                    <DataGrid ItemsSource="{Binding Produkty}">

                    </DataGrid>
                    <Separator/>
                    <DataGrid
                        ItemsSource="{Binding Produkty}"
                        CanUserDeleteRows="True"
                        CanUserReorderColumns="False"
                        CanUserSortColumns="False"
                        IsReadOnly="True"
                        AlternatingRowBackground="CadetBlue"
                        AlternationCount="3"
                        RowBackground="Beige"
                        >


                    </DataGrid>
                    <Separator/>
                    <DataGrid 
                    ItemsSource="{Binding Produkty}"
                        AutoGenerateColumns="False">
                        <DataGrid.Columns>
                            <DataGridTextColumn Header="Nazwa"
                                                Binding="{Binding Nazwa}"/>
                            <DataGridCheckBoxColumn Header="Dostępność"
                                                    Binding="{Binding Dostepny}"/>
                            <DataGridComboBoxColumn Header="Kategoria"
                                                    x:Name="kategoria_data_grid"
                                                    SelectedItemBinding="{Binding Kategoria}"
                                                    />
                        </DataGrid.Columns>
                    </DataGrid>


                </StackPanel>
            </TabItem>
        </TabControl>
    </Grid>
</Window>








using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace WpfApp1
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public List<String> ListaSlow { get; set; }
        public List<Produkt> Produkty { get; set; }
        public Produkt ZaznaczonyElement { get; set; }
        public MainWindow()
        {
            ListaSlow = new List<String>();
            ListaSlow.Add("programisci");
            ListaSlow.Add("nie");
            ListaSlow.Add("obijają");
            ListaSlow.Add("się");
            ListaSlow.Add("nawet");
            ListaSlow.Add("w");
            InitializeComponent();
            DataContext = this;
            lista3.ItemsSource = ListaSlow;
            przygotujDane();

        }
        private void przygotujDane()
        {
            Produkty = new List<Produkt>();
            Produkty.Add(new Produkt("klocki", 34.99, "plastikowe ", true));
            Produkty.Add(new Produkt("miś", 59.99, "milutki ", true));
            Produkty.Add(new Produkt("autko", 4.99, "szybkie ", true));
            Produkty.Add(new Produkt("samolot", 44, "", false));
            Produkty.Add(new Produkt("lalka", 70, "płacze je itd", true, "Dla dziewczynek"));
            Produkty.Add(new Produkt("nerf", 60, "strzela daleko", true, "Dla chłopców"));
            kategoria_data_grid.ItemsSource = new List<string>() { "Dla każdego", "Dla chłopców", "Dla dziewczynek" };
        }
    }
}






using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace WpfApp1
{
    public class Produkt
    {
        public string Nazwa { get; set; }
        public double Cena { get; set; }
        public string Opis { get; set; }
        public bool Dostepny { get; set; }
        public String Kategoria { get; set; }

        public Produkt(string nazwa, double cena, string opis, bool dostepny)
        {
            Nazwa = nazwa;
            Cena = cena;
            Opis = opis;
            Dostepny = dostepny;
            Kategoria = "Dla każdego";
        }

        public Produkt(string nazwa, double cena, string opis, bool dostepny, string kategoria) : this(nazwa, cena, opis, dostepny)
        {
            Kategoria = kategoria;
        }
    }
}
