{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Working with Known JSON Schemas - Lab"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Introduction\n",
    "In this lab, you'll practice working with JSON files whose schema you know beforehand."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Objectives\n",
    "You will be able to:\n",
    "* Read JSON Documentation Schemas and translate into code\n",
    "* Extract data from known JSON schemas\n",
    "* Write data to predefined JSON schemas"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Reading a JSON Schema\n",
    "\n",
    "Here's the JSON schema provided for a section of the NY Times API:\n",
    "<img src=\"images/nytimes_movie_schema.png\" width=500>\n",
    "\n",
    "or a fully expanded view:\n",
    "\n",
    "<img src=\"images/nytimes_movie_schema_detailed.png\" width=500>\n",
    "\n",
    "You can more about the documentation [here](https://developer.nytimes.com/docs/movie-reviews-api/1/routes/reviews/%7Btype%7D.json/get)."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "You can see that the master structure is a dictionary and has a key named 'response'. This is also a dictionary and has two keys: 'data' and 'meta'. As you continue to examine the schema hierarchy, you'll notice the vast majority, in this case, are dictionaries. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Loading the Data File\n",
    "\n",
    "Start by importing the json file. The sample response from the api is stored in a file **ny_times_movies.json**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "import json\n",
    "import pandas as pd\n",
    "\n",
    "with open(\"ny_times_movies.json\") as f:\n",
    "    d = json.load(f)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [],
   "source": [
    "def dict_method_to_list(method):\n",
    "    return list(map(lambda x : x, method))\n",
    "\n",
    "#### JSON Key chec, outputing names and file types\n",
    "def key_check(data):\n",
    "    \n",
    "    for level in data.keys():\n",
    "        \n",
    "        print(level, type(data[level]))\n",
    "        if type(data[level]) == dict:\n",
    "            print('-------',level,'dict')\n",
    "            \n",
    "            key_check(data[level])\n",
    "        if  type(data[level]) == list:\n",
    "            print('-------', level, 'list', type(data[level][0]), len(data[level]), len(data[level][0]))\n",
    "\n",
    "\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Loading Specific Data\n",
    "\n",
    "Create a DataFrame of the major data container within the json file, listed under the 'results' heading in the schema above."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "status <class 'str'>\n",
      "copyright <class 'str'>\n",
      "has_more <class 'bool'>\n",
      "num_results <class 'int'>\n",
      "results <class 'list'>\n",
      "------- results list <class 'dict'> 20 11\n"
     ]
    }
   ],
   "source": [
    "key_check(d)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>byline</th>\n",
       "      <th>critics_pick</th>\n",
       "      <th>date_updated</th>\n",
       "      <th>display_title</th>\n",
       "      <th>headline</th>\n",
       "      <th>link</th>\n",
       "      <th>mpaa_rating</th>\n",
       "      <th>multimedia</th>\n",
       "      <th>opening_date</th>\n",
       "      <th>publication_date</th>\n",
       "      <th>summary_short</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>A.O. SCOTT</td>\n",
       "      <td>1</td>\n",
       "      <td>2018-10-17 02:44:23</td>\n",
       "      <td>Can You Ever Forgive Me</td>\n",
       "      <td>Review: Melissa McCarthy Is Criminally Good in...</td>\n",
       "      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>\n",
       "      <td>R</td>\n",
       "      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>\n",
       "      <td>2018-10-19</td>\n",
       "      <td>2018-10-16</td>\n",
       "      <td>Marielle Heller directs a true story of litera...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>BEN KENIGSBERG</td>\n",
       "      <td>1</td>\n",
       "      <td>2018-10-16 11:04:03</td>\n",
       "      <td>Charm City</td>\n",
       "      <td>Review: â€˜Charm Cityâ€™ Vividly Captures the ...</td>\n",
       "      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>\n",
       "      <td></td>\n",
       "      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>\n",
       "      <td>2018-04-22</td>\n",
       "      <td>2018-10-16</td>\n",
       "      <td>Marilyn Nessâ€™s documentary is dedicated to t...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>GLENN KENNY</td>\n",
       "      <td>1</td>\n",
       "      <td>2018-10-16 11:04:04</td>\n",
       "      <td>Horn from the Heart: The Paul Butterfield Story</td>\n",
       "      <td>Review: Paul Butterfieldâ€™s Story Is Told in ...</td>\n",
       "      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>\n",
       "      <td></td>\n",
       "      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>\n",
       "      <td>2018-10-19</td>\n",
       "      <td>2018-10-16</td>\n",
       "      <td>A documentary explores the life of the blues m...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>A.O. SCOTT</td>\n",
       "      <td>0</td>\n",
       "      <td>2018-10-16 16:08:03</td>\n",
       "      <td>The Price of Everything</td>\n",
       "      <td>Review: â€˜The Price of Everythingâ€™ Asks $56...</td>\n",
       "      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>\n",
       "      <td></td>\n",
       "      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>\n",
       "      <td>2018-10-19</td>\n",
       "      <td>2018-10-16</td>\n",
       "      <td>This documentary examines the global art marke...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>BEN KENIGSBERG</td>\n",
       "      <td>0</td>\n",
       "      <td>2018-10-16 11:04:03</td>\n",
       "      <td>Impulso</td>\n",
       "      <td>Review: â€˜Impulsoâ€™ Goes Backstage With a Fl...</td>\n",
       "      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>\n",
       "      <td></td>\n",
       "      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>\n",
       "      <td>None</td>\n",
       "      <td>2018-10-16</td>\n",
       "      <td>This documentary follows RocÃ­o Molina, a cutt...</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "           byline  critics_pick         date_updated  \\\n",
       "0      A.O. SCOTT             1  2018-10-17 02:44:23   \n",
       "1  BEN KENIGSBERG             1  2018-10-16 11:04:03   \n",
       "2     GLENN KENNY             1  2018-10-16 11:04:04   \n",
       "3      A.O. SCOTT             0  2018-10-16 16:08:03   \n",
       "4  BEN KENIGSBERG             0  2018-10-16 11:04:03   \n",
       "\n",
       "                                     display_title  \\\n",
       "0                          Can You Ever Forgive Me   \n",
       "1                                       Charm City   \n",
       "2  Horn from the Heart: The Paul Butterfield Story   \n",
       "3                          The Price of Everything   \n",
       "4                                          Impulso   \n",
       "\n",
       "                                            headline  \\\n",
       "0  Review: Melissa McCarthy Is Criminally Good in...   \n",
       "1  Review: â€˜Charm Cityâ€™ Vividly Captures the ...   \n",
       "2  Review: Paul Butterfieldâ€™s Story Is Told in ...   \n",
       "3  Review: â€˜The Price of Everythingâ€™ Asks $56...   \n",
       "4  Review: â€˜Impulsoâ€™ Goes Backstage With a Fl...   \n",
       "\n",
       "                                                link mpaa_rating  \\\n",
       "0  {'type': 'article', 'url': 'http://www.nytimes...           R   \n",
       "1  {'type': 'article', 'url': 'http://www.nytimes...               \n",
       "2  {'type': 'article', 'url': 'http://www.nytimes...               \n",
       "3  {'type': 'article', 'url': 'http://www.nytimes...               \n",
       "4  {'type': 'article', 'url': 'http://www.nytimes...               \n",
       "\n",
       "                                          multimedia opening_date  \\\n",
       "0  {'type': 'mediumThreeByTwo210', 'src': 'https:...   2018-10-19   \n",
       "1  {'type': 'mediumThreeByTwo210', 'src': 'https:...   2018-04-22   \n",
       "2  {'type': 'mediumThreeByTwo210', 'src': 'https:...   2018-10-19   \n",
       "3  {'type': 'mediumThreeByTwo210', 'src': 'https:...   2018-10-19   \n",
       "4  {'type': 'mediumThreeByTwo210', 'src': 'https:...         None   \n",
       "\n",
       "  publication_date                                      summary_short  \n",
       "0       2018-10-16  Marielle Heller directs a true story of litera...  \n",
       "1       2018-10-16  Marilyn Nessâ€™s documentary is dedicated to t...  \n",
       "2       2018-10-16  A documentary explores the life of the blues m...  \n",
       "3       2018-10-16  This documentary examines the global art marke...  \n",
       "4       2018-10-16  This documentary follows RocÃ­o Molina, a cutt...  "
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df = pd.DataFrame(d['results'])\n",
    "df.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## How many unique critics are there?"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "['A.O. SCOTT' 'BEN KENIGSBERG' 'GLENN KENNY' 'JEANNETTE CATSOULIS'\n",
      " 'MANOHLA DARGIS' 'KEN JAWOROWSKI' 'TEO BUGBEE']\n",
      "7\n"
     ]
    }
   ],
   "source": [
    "print(df.byline.unique())\n",
    "print(df.byline.nunique())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Create a new column for the review's url. Title the column 'review_url'"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [],
   "source": [
    "df['url'] = df.link.apply(lambda x: x['url'])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## How many results are in the file?"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "20"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(df)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Summary\n",
    "Well done! Here you continued to gather practice extracting data from JSON files and transforming them into our standard tool of Pandas DataFrames."
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.6"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
