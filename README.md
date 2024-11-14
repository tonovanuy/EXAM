public class Song
{
    public string Title { get; set; }
    public int Votes { get; set; }

    public Song(string title)
    {
        Title = title;
        Votes = 0;
    }
    
}
using System.Collections.Generic;
using System.Linq;

public class VoteManager
{
    public List<Song> Songs { get; set; }

    public VoteManager()
    {
        Songs = new List<Song>
        {
            new Song("Diva (Israel)"),
            new Song("Satellite (Germany)"),
            new Song("Toy (Israel)")
        };
    }

    // Add voice
    public void AddVote(int songIndex)
    {
        Songs[songIndex].Votes++;
    }

    // Get the song with the most votes
    public Song GetWinner()
    {
        return Songs.OrderByDescending(s => s.Votes).First();
    }
}

<Window x:Class="SimpleEurovisionApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Eurovision Voting" Height="300" Width="400">
    <Grid>
        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Top" FontSize="20" Margin="10" Text="Vote for your favorite song!"/>

        <!-- List of songs -->
        <ComboBox Name="SongsComboBox" HorizontalAlignment="Center" VerticalAlignment="Top" Margin="10,50,10,0" Width="300"/>

        <!-- Vote button -->
        <Button Name="VoteButton" Content="Vote" HorizontalAlignment="Center" VerticalAlignment="Top" Width="100" Margin="10,120,10,0" Click="VoteButton_Click"/>

        <!-- Output of results -->
        <TextBlock Name="ResultsTextBlock" HorizontalAlignment="Center" VerticalAlignment="Top" Margin="10,180,10,0" FontSize="16"/>
    </Grid>
</Window>

using System.Windows;

namespace SimpleEurovisionApp
{
    public partial class MainWindow : Window
    {
        private VoteManager _voteManager;

        public MainWindow()
        {
            InitializeComponent();
            _voteManager = new VoteManager();
            LoadSongs();
        }

        // Download songs to ComboBox
        private void LoadSongs()
        {
            foreach (var song in _voteManager.Songs)
            {
                SongsComboBox.Items.Add(song.Title);
            }
        }

        // Voting processing
        private void VoteButton_Click(object sender, RoutedEventArgs e)
        {
            int selectedIndex = SongsComboBox.SelectedIndex;

            if (selectedIndex >= 0)
            {
                _voteManager.AddVote(selectedIndex);
                Song winner = _voteManager.GetWinner();
                ResultsTextBlock.Text = $"Most voted song: {winner.Title} with {winner.Votes} votes";
            }
            else
            {
                MessageBox.Show("Please select a song to vote.");
            }
        }
    }
}
