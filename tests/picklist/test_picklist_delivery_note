#Submitting a picklist and creating a delivery note for it, 
# then verifying the status is Open.


import os
from playwright.sync_api import sync_playwright
from dotenv import load_dotenv
import pytest

load_dotenv()
 
@pytest.mark.picklist
@pytest.mark.regression
def test_picklist_status_after_creating_picklist():
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=True, slow_mo=800)
        page = browser.new_page()
        page.set_default_timeout(30000)

        page.goto("https://test-vidirav15.frappe.cloud/login")


        page.fill("#login_email", os.getenv("ERP_USER"))
        page.fill("#login_password", os.getenv("ERP_PASS"))
        page.click("button:has-text('Login')")


        page.wait_for_load_state("networkidle")
        page.wait_for_timeout(3000)
        page.locator("a").filter(has_text="ERPNext").click()
        
        

        page.get_by_role("link", name="Selling").click()

        page.get_by_role("link", name="Book Order").click()

        page.wait_for_selector("text=Book Order")

        print("Book Order page opened")
        
        page.get_by_role("button", name="Add Book Order").click()
        
        
        
        page.locator('input[data-fieldname="customer"]').fill("C")
        page.locator('div[role="option"]').first.click()
        page.locator(".btn.btn-modal-close").click()
        page.locator('input[data-fieldname="set_warehouse"]').fill("WH-MAIN - VIPL")
        page.keyboard.press("Enter")
        
        if page.locator(".modal-content").is_visible():
            page.locator(".btn-modal-close").click()
            page.wait_for_selector(".modal-backdrop", state="detached")

        page.locator('.grid-row[data-idx="1"] [data-fieldname="product_code"].error.bold').click()
        
        page.get_by_role("combobox", name="Product Code").fill("SAP0047TL")
        page.wait_for_timeout(3000)
        page.get_by_role("combobox", name="Product Code").press("Enter")
        
        page.wait_for_timeout(5000)
        
        page.get_by_role("button", name="Save").click()
        page.get_by_role("button", name="Submit").click()
        page.get_by_role("button", name="Yes").click()
        page.wait_for_timeout(3000)
        page.locator(".btn.btn-modal-close").first.click()
        page.get_by_role("button", name="Pick List").click()
        page.get_by_role("button",name="Save").click()
        page.wait_for_timeout(3000)
        picklist_id = page.get_by_role("listitem").filter(has_text="VI-PICK#").inner_text()
        print(picklist_id)
        page.get_by_role("button",name="Submit").click()
        page.wait_for_timeout(3000)
        page.get_by_role("button", name= "Yes").click()
        
        page.wait_for_timeout(2000)
        page.locator(".btn.btn-modal-close").first.click() 
        page.get_by_role("button", name="Create").click()
        page.get_by_role("link", name="Create Delivery Note").click()
        page.locator(".btn.btn-modal-close").first.click() 
        page.wait_for_timeout(2000)
        page.locator("#delivery-note-__details > div:nth-child(2) > .section-head > .ml-2 > .es-icon").click()
        page.wait_for_timeout(3000)
        page.locator('input[data-fieldname="cost_center"]').click()
        page.get_by_role("option", name="Main - VIPL").click()
        page.wait_for_timeout(2000)
        page.get_by_role("button", name="Save").click()
        page.wait_for_timeout(2000)
        page.get_by_role("button", name="Submit").click()
        page.wait_for_timeout(2000)
        page.get_by_role("button", name="Yes").click()
        page.wait_for_timeout(2000)
        page.get_by_role("link", name="App Logo").click()
        page.wait_for_timeout(2000)
        page.get_by_role("link", name="Stock", exact=True).click()
        page.wait_for_timeout(2000)
        page.get_by_role("link", name="Pick List", exact=True).click()

        page.wait_for_selector("text=Pick List")
        page.get_by_role("button", name="Clear all filters").click()
        
        page.get_by_role("textbox", name="ID").click()
        page.get_by_role("textbox", name="ID").fill(picklist_id)
        page.get_by_role("link", name=picklist_id).click()
        status = page.locator('span.indicator-pill span:visible').inner_text().strip()
        print(status)
        assert status== "Completed", f"Status {status} is not 'Completed'"
        browser.close()
        
