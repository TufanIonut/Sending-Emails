using MailKit.Net.Smtp;
using MailKit.Security;
using MimeKit;

public class EmailService
{
    private readonly string smtpServer;
    private readonly int smtpPort;
    private readonly string smtpUsername;
    private readonly string smtpPassword;

    public EmailService(string server, int port, string username, string password)
    {
        smtpServer = server;
        smtpPort = port;
        smtpUsername = username;
        smtpPassword = password;
    }

    public void SendCampaignFinishedEmail(string recipientEmail, string campaignName)
    {
        var message = new MimeMessage();
        message.From.Add(new MailboxAddress("Your Name", "your@email.com"));
        message.To.Add(new MailboxAddress("", recipientEmail));
        message.Subject = $"Campaign '{campaignName}' has finished.";

        message.Body = new TextPart("plain")
        {
            Text = "Your campaign has successfully finished! Thank you for your participation."
        };

        using (var client = new SmtpClient())
        {
            client.Connect(smtpServer, smtpPort, SecureSocketOptions.StartTls);
            client.Authenticate(smtpUsername, smtpPassword);
            client.Send(message);
            client.Disconnect(true);
        }
    }
}
![image](https://github.com/TufanIonut/Sending-Emails/assets/117408976/c96c7d8a-92b7-4115-9e70-eb8ecdee1237)
