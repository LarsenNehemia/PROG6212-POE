# PROG6212-POE

READ ME:

In order to run the code the user needs to have visual studio 2022 installed on either Windows 8-11 or MAC os 
the application is a sysstem claim application that allows lectuers to insert claims and each of their claims can either be approved or reject by the manager.
the application has 2 sides, one being the lecturer's dashboard and the Managers dashboard.

XAML CODE

<Window x:Class="CMCS.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Contract Monthly Claim System" Height="600" Width="800" Background="#F0F8FF">
    <Grid>
        <TabControl>
            <!-- Lecturer Dashboard Tab -->
            <TabItem Header="Lecturer Dashboard">
                <Grid>
                    <TextBlock Text="Submit Monthly Claim" FontSize="20" FontWeight="Bold" Margin="10,10,0,0" HorizontalAlignment="Left"/>
                    <StackPanel Orientation="Vertical" HorizontalAlignment="Left" Margin="10,50,0,0">
                        <Label Content="Lecturer ID:"/>
                        <TextBox Name="LecturerIDTextBox_L" Width="200"/>
                        <Label Content="Lecturer Name:"/>
                        <TextBox Name="LecturerNameTextBox_L" Width="200"/>
                        <Label Content="Hours Worked:"/>
                        <TextBox Name="HoursWorkedTextBox_L" Width="200"/>
                        <Label Content="Hourly Rate:"/>
                        <TextBox Name="HourlyRateTextBox_L" Width="200"/>
                        <!-- New TextBox for Additional Notes -->
                        <Label Content="Additional Notes:"/>
                        <TextBox Name="AdditionalNotesTextBox_L" Width="200" Height="100" TextWrapping="Wrap" AcceptsReturn="True"/>
                        <Button Content="Upload Document" Width="150" Margin="0,10,0,0" Background="#ADD8E6" Click="UploadDocument_Click_L"/>
                        <Button Content="Submit Claim" Width="150" Margin="0,10,0,0" Background="#32CD32" Click="SubmitClaim_Click_L"/>
                    </StackPanel>
                    <TextBlock Text="Your Claims Status" FontSize="16" FontWeight="Bold" Margin="10,300,0,0" HorizontalAlignment="Left"/>
                    <TextBox Name="ClaimsStatusTextBox_L" Width="750" Height="100" Margin="10,330,0,0" IsReadOnly="True" Background="#FFFACD"/>
                </Grid>
            </TabItem>
            <!-- Coordinator/Manager Dashboard Tab -->
            <TabItem Header="Coordinator/Manager Dashboard">
                <Grid>
                    <TextBlock Text="Verify and Approve Claims" FontSize="20" FontWeight="Bold" Margin="10,10,0,0" HorizontalAlignment="Left"/>
                    <StackPanel Orientation="Vertical" HorizontalAlignment="Left" Margin="10,50,0,0">
                        <Label Content="Lecturer ID:"/>
                        <TextBox Name="LecturerIDTextBox_C" Width="200" IsReadOnly="True"/>
                        <Label Content="Lecturer Name:"/>
                        <TextBox Name="LecturerNameTextBox_C" Width="200" IsReadOnly="True"/>
                        <Label Content="Hours Worked:"/>
                        <TextBox Name="HoursWorkedTextBox_C" Width="200" IsReadOnly="True"/>
                        <Label Content="Claim Amount:"/>
                        <TextBox Name="ClaimAmountTextBox_C" Width="200" IsReadOnly="True"/>
                        <Label Content="Claim Status:"/>
                        <Label Name="ClaimStatusLabel_C" Width="200" FontSize="16" FontWeight="Bold" Content="Pending" Foreground="DarkBlue"/>
                        <!-- New TextBox for Additional Notes (ReadOnly) -->
                        <Label Content="Lecturer's Notes:"/>
                        <TextBox Name="AdditionalNotesTextBox_C" Width="200" Height="100" TextWrapping="Wrap" IsReadOnly="True" Background="#FFFACD"/>
                        <Button Content="Approve Claim" Width="150" Margin="0,10,0,0" Background="#32CD32" Click="ApproveClaim_Click"/>
                        <Button Content="Reject Claim" Width="150" Margin="0,10,0,0" Background="#FF6347" Click="RejectClaim_Click"/>
                        <!-- Progress Bar -->
                        <Label Content="Progress:"/>
                        <ProgressBar Name="ProgressBar_C" Width="200" Height="20" Value="33"/>
                    </StackPanel>
                    <TextBlock Text="All Claims" FontSize="16" FontWeight="Bold" Margin="10,300,0,0" HorizontalAlignment="Left"/>
                    <DataGrid Name="ClaimsDataGrid_C" Width="750" Height="200" Margin="10,330,0,0" ItemsSource="{Binding claims}" IsReadOnly="True"/>
                </Grid>
            </TabItem>
        </TabControl>
    </Grid>
</Window>


XAML CS CODE:

using System.Collections.Generic;
using System.Linq;
using System.Windows;
using System.Windows.Controls;

namespace CMCS
{
    public partial class MainWindow : Window
    {
        // List to hold submitted claims
        private List<Claim> claims = new List<Claim>();
        public MainWindow()
        {
            InitializeComponent();
            LoadPendingClaims();  // For the Coordinator/Manager Dashboard
            UpdateClaimList();    // Populate the Claims DataGrid with all claims
        }
        // -------------------- Lecturer Dashboard Logic ------------------------
        private void SubmitClaim_Click_L(object sender, RoutedEventArgs e)
        {
            string lecturerID = LecturerIDTextBox_L.Text;
            string lecturerName = LecturerNameTextBox_L.Text;
            string hoursWorked = HoursWorkedTextBox_L.Text;
            string hourlyRate = HourlyRateTextBox_L.Text;
            string additionalNotes = AdditionalNotesTextBox_L.Text; // Get lecturer's notes
            if (string.IsNullOrEmpty(lecturerID) || string.IsNullOrEmpty(lecturerName) || string.IsNullOrEmpty(hoursWorked) || string.IsNullOrEmpty(hourlyRate))
            {
                MessageBox.Show("Please fill all fields before submitting the claim.");
                return;
            }
            // Create a new claim with additional notes and add it to the list
            var newClaim = new Claim(lecturerID, lecturerName, hoursWorked, hourlyRate, additionalNotes);
            claims.Add(newClaim);
            ClaimsStatusTextBox_L.Text = "Claim successfully submitted with notes!";
            UpdateClaimList();  // Update the list of all claims in real-time
        }
        // -------------------- Coordinator Dashboard Logic ---------------------
        private void LoadPendingClaims()
        {
            if (claims.Any(c => c.ClaimStatus == "Pending"))
            {
                // Load the first pending claim into the dashboard
                var claim = claims.FirstOrDefault(c => c.ClaimStatus == "Pending");
                LecturerIDTextBox_C.Text = claim.LecturerID;
                LecturerNameTextBox_C.Text = claim.LecturerName;
                HoursWorkedTextBox_C.Text = claim.HoursWorked;
                ClaimAmountTextBox_C.Text = claim.ClaimAmount;
                ClaimStatusLabel_C.Content = claim.ClaimStatus;
 AdditionalNotesTextBox_C.Text = claim.AdditionalNotes; // Display notes on manager dashboard
                UpdateProgressBar(claim.ClaimStatus);
            }
            else
            {
                // If no pending claims, clear the fields
                LecturerIDTextBox_C.Text = "";
                LecturerNameTextBox_C.Text = "";
                HoursWorkedTextBox_C.Text = "";
                ClaimAmountTextBox_C.Text = "";
                ClaimStatusLabel_C.Content = "No pending claims.";
                AdditionalNotesTextBox_C.Text = ""; // Clear notes field
                ProgressBar_C.Value = 0;
            }
        }
        private void ApproveClaim_Click(object sender, RoutedEventArgs e)
        {
            UpdateClaimStatus("Approved");
        }
        private void RejectClaim_Click(object sender, RoutedEventArgs e)
        {
            UpdateClaimStatus("Rejected");
        }
        private void UpdateClaimStatus(string newStatus)
        {
            if (claims.Any(c => c.ClaimStatus == "Pending"))
            {
                var claim = claims.FirstOrDefault(c => c.ClaimStatus == "Pending");
                claim.ClaimStatus = newStatus;
                ClaimStatusLabel_C.Content = newStatus;
                UpdateProgressBar(newStatus);
                MessageBox.Show($"Claim status updated to: {newStatus}");
// Update the claim list in real-time after status change
                UpdateClaimList();
                // Optionally, load the next pending claim (if any)
                LoadPendingClaims();
            }
            else
            {
                MessageBox.Show("No claims to process.");
            }
        }
        private void UpdateProgressBar(string status)
        {
            switch (status)
            {
                case "Pending":
                    ProgressBar_C.Value = 33;
                    break;
                case "Approved":
                    ProgressBar_C.Value = 100;
                    break;
                case "Rejected":
                    ProgressBar_C.Value = 100;
                    break;
                default:
                    ProgressBar_C.Value = 0;
                    break;
            }
        }
        // -------------------- View All Claims Logic ---------------------
        private void UpdateClaimList()
        {
            ClaimsDataGrid_C.ItemsSource = null;
            ClaimsDataGrid_C.ItemsSource = claims;
        }
    }
    public class Claim
    {
        public string LecturerID { get; set; }
        public string LecturerName { get; set; }
        public string HoursWorked { get; set; }
        public string ClaimAmount { get; set; }
        public string ClaimStatus { get; set; }
        public string AdditionalNotes { get; set; }  // New field for additional notes
        public Claim(string lecturerID, string lecturerName, string hoursWorked, string hourlyRate, string additionalNotes)
        {
            LecturerID = lecturerID;
            LecturerName = lecturerName;
            HoursWorked = hoursWorked;
            ClaimAmount = (int.Parse(hoursWorked) * double.Parse(hourlyRate)).ToString();
            ClaimStatus = "Pending";
            AdditionalNotes = additionalNotes;  // Store lecturer's notes
        }
    }
}

