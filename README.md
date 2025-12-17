# YouTube Sentiment Analyzer 🎬📊

A comprehensive sentiment analysis pipeline for YouTube comments featuring a machine learning model, Flask API, and Chrome extension interface for real-time analysis.

## 🌟 Features

### 🚀 Core Functionality
- **Real-time Sentiment Analysis**: Analyze YouTube comments in real-time with sentiment classification (Positive/Neutral/Negative)
- **Chrome Extension**: Browser extension that extracts and analyzes comments directly from YouTube videos
- **Interactive Visualizations**: Generate pie charts, word clouds, and trend graphs from comment data
- **RESTful API**: Flask backend serving predictions and visualizations
- **ML Pipeline**: Complete ML workflow from data ingestion to model deployment

### 📈 Analytics & Visualizations
- **Sentiment Distribution**: Pie chart showing sentiment breakdown
- **Trend Analysis**: Monthly sentiment trends over time
- **Word Clouds**: Visual representation of most frequent words
- **Comment Metrics**: Total comments, unique commenters, average sentiment scores

## 🏗️ Project Architecture

```
youtube-sentiment-analyzer/
├── flask_app/                 # Flask API backend
│   ├── app.py                # Main Flask application
│   └── test.py               # Model testing script
├── src/                      # Machine learning pipeline
│   ├── data/                 # Data processing modules
│   │   ├── data_ingestion.py
│   │   └── data_preprocessing.py
│   └── model/                # Model building and evaluation
│       ├── model_building.py
│       ├── model_evaluation.py
│       └── register_model.py
├── yt-chrome-plugin-frontend/# Chrome extension
│   ├── manifest.json         # Extension configuration
│   ├── popup.html           # Extension UI
│   └── popup.js             # Extension logic
├── notebooks/                # Jupyter notebooks for experimentation
├── data/                     # Data storage (git-ignored)
├── models/                   # Trained models (git-ignored)
└── config/                   # Configuration files
```

## 🛠️ Installation & Setup

### Prerequisites
- Python 3.8+
- Google Chrome browser
- YouTube Data API key

### 1. Backend Setup

```bash
# Clone the repository
git clone <repository-url>
cd youtube-sentiment-analyzer

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Download NLTK data
python -c "import nltk; nltk.download('stopwords'); nltk.download('wordnet')"
```

### 2. Chrome Extension Setup

1. Open Chrome and navigate to `chrome://extensions/`
2. Enable "Developer mode"
3. Click "Load unpacked"
4. Select the `yt-chrome-plugin-frontend` directory
5. Update the API key in `popup.js` with your YouTube Data API key

### 3. YouTube Data API Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing
3. Enable YouTube Data API v3
4. Create credentials (API Key)
5. Add the key to `yt-chrome-plugin-frontend/popup.js`

## 📊 Machine Learning Pipeline

### Data Processing Workflow

1. **Data Ingestion**: Load and split Reddit data (used for training)
2. **Preprocessing**: 
   - Text normalization
   - Stopword removal
   - Lemmatization
   - Special character handling
3. **Feature Engineering**: TF-IDF with n-grams (1-3)
4. **Model Training**: LightGBM with hyperparameter optimization
5. **Model Evaluation**: Classification metrics and confusion matrix
6. **Model Registration**: MLflow model registry integration

### Model Training

```bash
# Run the complete pipeline
dvc repro

# Or run individual stages
python src/data/data_ingestion.py
python src/data/data_preprocessing.py
python src/model/model_building.py
python src/model/model_evaluation.py
```

## 🚀 Usage

### Starting the Flask API

```bash
cd flask_app
python app.py
# Server runs on http://localhost:5000
```

### Using the Chrome Extension

1. Navigate to any YouTube video
2. Click the extension icon in Chrome toolbar
3. View sentiment analysis results including:
   - Sentiment distribution chart
   - Monthly trend graph
   - Word cloud
   - Top comments with sentiments
   - Analytics metrics

### API Endpoints

| Endpoint | Method | Description |
|----------|---------|-------------|
| `/predict` | POST | Predict sentiment for list of comments |
| `/predict_with_timestamps` | POST | Predict with timestamps for trend analysis |
| `/generate_chart` | POST | Generate sentiment pie chart |
| `/generate_wordcloud` | POST | Generate word cloud from comments |
| `/generate_trend_graph` | POST | Generate sentiment trend graph |

## 🔧 Configuration

### `params.yaml`
```yaml
data_ingestion:
  test_size: 0.20

model_building:
  ngram_range: [1, 3]
  max_features: 1000
  learning_rate: 0.09
  max_depth: 20
  n_estimators: 367
```

### Environment Variables
- `MLFLOW_TRACKING_URI`: MLflow server URL
- `YOUTUBE_API_KEY`: YouTube Data API key

## 📁 Project Structure Details

### `flask_app/app.py`
Main Flask application with endpoints for:
- Sentiment prediction
- Chart generation
- Word cloud creation
- Trend analysis

### `src/data/data_ingestion.py`
- Loads data from Reddit dataset
- Splits into train/test sets
- Handles missing values and duplicates

### `src/data/data_preprocessing.py`
- Text cleaning and normalization
- Stopword removal (preserving negation words)
- Lemmatization

### `src/model/model_building.py`
- TF-IDF vectorization with n-grams
- LightGBM classifier training
- Model serialization

### `src/model/model_evaluation.py`
- Model performance evaluation
- MLflow experiment tracking
- Confusion matrix generation

### `yt-chrome-plugin-frontend/`
- Chrome extension UI and logic
- YouTube API integration
- Real-time comment fetching

## 📊 Model Performance

The LightGBM model achieves:
- Multi-class classification (Positive/Neutral/Negative)
- TF-IDF features with trigrams
- Optimized hyperparameters through grid search
- MLflow experiment tracking

## 🔍 Testing

```bash
# Test the model locally
cd flask_app
python test.py

# Test API endpoints
curl -X POST http://localhost:5000/predict \
  -H "Content-Type: application/json" \
  -d '{"comments": ["I love this video!", "This is terrible"]}'
```

## 🚢 Deployment

### Backend Deployment
1. **Local**: Run Flask app directly
2. **Docker**: Containerize the application
3. **Cloud**: Deploy to AWS/GCP/Azure with load balancing

### Model Deployment Options
1. **MLflow Model Registry**: Version and stage management
2. **Docker**: Containerized model serving
3. **Serverless**: AWS Lambda or GCP Cloud Functions

## 🔮 Future Enhancements

### Planned Features
- [ ] Multi-language support
- [ ] Advanced visualization dashboards
- [ ] Real-time streaming analysis
- [ ] User authentication and history
- [ ] Advanced NLP techniques (BERT, RoBERTa)
- [ ] Automated retraining pipeline
- [ ] Alert system for sentiment spikes

### Technical Improvements
- [ ] Async processing for large comment volumes
- [ ] Caching for frequent video analysis
- [ ] Database integration for result storage
- [ ] Advanced error handling and logging
- [ ] A/B testing for model improvements

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 👥 Authors

- **Charlie** - Initial work - [charlieduyilemi@yahoo.com](mailto:charlieduyilemi@yahoo.com)

## 🙏 Acknowledgments

- YouTube Data API for comment fetching
- LightGBM for efficient gradient boosting
- MLflow for experiment tracking
- NLTK for NLP preprocessing
- DVC for data version control

## 📧 Contact

For questions or feedback, please contact:
- Email: charlieduyilemi@yahoo.com
- GitHub Issues: [Project Issues](https://github.com/duyilemi/YouTube-Sentiment-Analyzer/issues)

---

⭐ **Star this repo if you found it useful!** ⭐