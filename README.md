# PROG6212-POE

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
