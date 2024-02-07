from telethon import TelegramClient, events

api_id = 24779532  # Replace with your actual Telegram API ID
api_hash = 'a06c2f7af2c9f86a96e2f8ebfaf56050'  # Replace with your actual Telegram API hash
phone_number = '+919540783080'  # Include the country code

source_channel = -1002007397642  # Replace with your actual source channel ID
destination_channels = [-1002088482863]  # Replace with your actual destination channel IDs
other_destination_channel = -1002007397642  # Replace with the actual ID of your other destination channel

# Link replacements
link_replacements = {
    "https://bigdaddygame.in/#/register?r_code=t5fow400885": "https://bigdaddygame.in/#/register?invitationCode=445772867517",
    "https://www.9987up.co/#/register?r_code=VkKlF31190": "",  # Empty string to delete the link
}

# Your Instagram link for YouTube link replacement
instagram_link = "https://www.instagram.com/just.earningmoney?igsh=MW9pcjd0dXMwaWtjaw=="

# Define additional_links_to_delete as a list of links to be removed from the message
additional_links_to_delete = [
    "https://example.com",
    "https://anotherexample.com",
]

client = TelegramClient('session_name', api_id, api_hash)

@client.on(events.NewMessage(outgoing=False))
async def my_event_handler(event):
    if event.chat_id == source_channel:
        clipboard = event.message.message

        # Replace specific links with replacements
        for link, replacement in link_replacements.items():
            clipboard = clipboard.replace(link, replacement)

        # Detect and replace YouTube links with the Instagram link
        clipboard = clipboard.replace("https://www.youtube.com/", instagram_link)

        # Detect and replace Instagram links with the Instagram link
        clipboard = clipboard.replace("https://www.instagram.com/", instagram_link)

        # Delete additional links from the message
        for link_to_delete in additional_links_to_delete:
            if link_to_delete in clipboard:
                clipboard = clipboard.replace(link_to_delete, "")

        # Forward the message to the appropriate destination channels
        forwarded = False
        for channel in destination_channels:
            await client.send_message(channel, clipboard)
            forwarded = True

        # If no specific link was found, forward the message to the other destination channel
        if not forwarded:
            await client.send_message(other_destination_channel, clipboard)

async def main():
    await client.start(phone=phone_number)

    # Your Telegram API operations go here
    me = await client.get_me()
    print(f"Logged in as: {me.stringify()}")

    # Continue with other Telegram API operations as needed

    await client.run_until_disconnected()

if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
