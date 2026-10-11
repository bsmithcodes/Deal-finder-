# Deal-finder-
I like technology but it’s sometimes expensive so i made deal finder a python script that finds deals below you will find the code 

import requests

def fetch_tech_deals(keyword="", limit=15):
    # Using Reddit's public JSON endpoint for tech deals
    url = "https://www.reddit.com/r/buildapcsales/new.json"
    headers = {"User-Agent": "TechDealFinderBot/1.0"}
    
    try:
        response = requests.get(url, headers=headers)
        response.raise_for_status()
    except requests.exceptions.RequestException as e:
        print(f"Error fetching data: {e}")
        return []

    data = response.json()
    posts = data.get("data", {}).get("children", [])
    
    deals = []
    for post in posts:
        p_data = post.get("data", {})
        title = p_data.get("title", "")
        deal_url = p_data.get("url", "")
        
        # Filter by keyword if provided
        if keyword and keyword.lower() not in title.lower():
            continue
            
        deals.append({
            "title": title,
            "url": deal_url
        })
        
        if len(deals) >= limit:
            break
            
    return deals

if __name__ == "__main__":
    print("--- Tech Deal Finder ---")
    search_keyword = input("Enter keyword to search (e.g., SSD, GPU, Laptop) or leave blank: ").strip()
    
    found_deals = fetch_tech_deals(keyword=search_keyword, limit=10)
    
    if not found_deals:
        print("No deals found matching your criteria.")
    else:
        print(f"\nFound {len(found_deals)} matching deals:\n")
        for i, deal in enumerate(found_deals, 1):
            print(f"{i}. {deal['title']}")
            print(f"   Link: {deal['url']}\n")
