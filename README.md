# fb_photo_download
Python project to leverage the Facebook API to download all photos from a facebook profile
Here's the readme formatted in proper markdown syntax:

```markdown
# Facebook Photo Backup to Google Photos

A Python tool that backs up your Facebook photos to Google Photos, preserving metadata like upload dates and photo tags.

## Features

* Download all photos from a Facebook profile
* Preserve photo metadata including:
  * Original upload date
  * Tagged people
  * Location data (if available)
  * Comments and reactions
* Automatic upload to Google Photos
* Progress tracking and resume capability
* Rate limiting to comply with API restrictions
* Error handling and logging

## Prerequisites

* Python 3.8+
* Facebook Developer Account
* Google Cloud Project with Photos API enabled
* Required Python packages (see `requirements.txt`)

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/facebook-photo-backup.git
   cd facebook-photo-backup
   ```

2. Install required packages:
   ```bash
   pip install -r requirements.txt
   ```

3. Set up your credentials:
   * Create a `.env` file with your API keys:
   ```env
   FACEBOOK_APP_ID=your_app_id
   FACEBOOK_APP_SECRET=your_app_secret
   GOOGLE_CLIENT_ID=your_client_id
   GOOGLE_CLIENT_SECRET=your_client_secret
   ```

## Configuration

### Facebook API Setup:
* Go to [Facebook Developers](https://developers.facebook.com)
* Create a new app
* Enable the Photos API
* Add your app's OAuth redirect URI
* Copy your App ID and App Secret to `.env`

### Google Photos API Setup:
* Go to [Google Cloud Console](https://console.cloud.google.com)
* Create a new project
* Enable the Photos Library API
* Create OAuth 2.0 credentials
* Download the client configuration file

## Usage

1. Run the main script:
   ```bash
   python photo_backup.py
   ```

2. First-time setup:
   * You'll be prompted to authenticate with Facebook
   * Then authenticate with Google Photos
   * Tokens will be saved for future use

3. Select backup options:
   ```bash
   python photo_backup.py --all  # Backup all photos
   python photo_backup.py --since YYYY-MM-DD  # Backup photos since date
   python photo_backup.py --album "Album Name"  # Backup specific album
   ```

## File Structure
```
facebook-photo-backup/
├── photo_backup.py
├── src/
│   ├── facebook_client.py
│   ├── google_photos_client.py
│   ├── metadata_handler.py
│   └── utils.py
├── tests/
│   ├── test_facebook_client.py
│   ├── test_google_photos_client.py
│   └── test_metadata_handler.py
├── config/
│   └── default_config.yml
├── requirements.txt
└── README.md
```

## Code Example

```python
from src.facebook_client import FacebookClient
from src.google_photos_client import GooglePhotosClient

# Initialize clients
fb_client = FacebookClient()
google_client = GooglePhotosClient()

# Download photos with metadata
photos = fb_client.get_all_photos()

# Process and upload each photo
for photo in photos:
    metadata = {
        'upload_date': photo.created_time,
        'tagged_people': photo.tags,
        'location': photo.place
    }
    
    google_client.upload_photo(
        photo.source,
        metadata=metadata
    )
```

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Error Handling

The script includes robust error handling for common issues:
* API rate limiting
* Network connectivity issues
* Authentication failures
* Insufficient permissions
* Corrupted or invalid photos


## Notes

* The Facebook Graph API has rate limits. The script implements exponential backoff.
* Some older photos might have incomplete metadata.
* The script requires appropriate Facebook permissions (user_photos, public_profile).
* Google Photos API has storage quotas and upload limits.

## Support

For support:
1. Check the [Issues](https://github.com/yourusername/facebook-photo-backup/issues) page
2. Review the [FAQ](docs/FAQ.md)
3. Create a new issue with:
   * Python version
   * Error message
   * Steps to reproduce
