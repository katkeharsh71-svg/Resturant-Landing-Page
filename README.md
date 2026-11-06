# Resturant-Landing-Page
/* =========================================================
   Kokum & Co. - Restaurant Landing Page
   Features: mobile nav, menu filter, gallery lightbox,
   reservation form validation
   ========================================================= */

// ---------- 1. Mobile navigation ----------
const navToggle = document.getElementById("navToggle");
const navLinks = document.getElementById("navLinks");

navToggle.addEventListener("click", () => {
  const isOpen = navLinks.classList.toggle("open");
  navToggle.setAttribute("aria-expanded", isOpen);
});
// Close the menu after tapping a link
navLinks.querySelectorAll("a").forEach((link) =>
  link.addEventListener("click", () => {
    navLinks.classList.remove("open");
    navToggle.setAttribute("aria-expanded", "false");
  })
);

// ---------- 2. Menu filter ----------
const filterButtons = document.querySelectorAll(".filter");
const cards = document.querySelectorAll(".card");

filterButtons.forEach((btn) => {
  btn.addEventListener("click", () => {
    filterButtons.forEach((b) => b.classList.remove("active"));
    btn.classList.add("active");

    const chosen = btn.dataset.filter;
    cards.forEach((card) => {
      const show = chosen === "all" || card.dataset.category === chosen;
      card.classList.toggle("hide", !show);
      if (show) {
        // restart the pop animation
        card.style.animation = "none";
        card.offsetHeight; // force reflow
        card.style.animation = "";
      }
    });
  });
});

// ---------- 3. Gallery lightbox ----------
const lightbox = document.getElementById("lightbox");
const lightboxArt = document.getElementById("lightboxArt");
const lightboxCaption = document.getElementById("lightboxCaption");

document.querySelectorAll(".tile").forEach((tile) => {
  tile.addEventListener("click", () => {
    lightboxArt.textContent = tile.textContent;
    lightboxArt.style.background = getComputedStyle(tile).backgroundImage;
    lightboxCaption.textContent = tile.dataset.label;
    lightbox.hidden = false;
    document.getElementById("lightboxClose").focus();
  });
});

function closeLightbox() {
  lightbox.hidden = true;
}
document.getElementById("lightboxClose").addEventListener("click", closeLightbox);
lightbox.addEventListener("click", (e) => {
  if (e.target === lightbox) closeLightbox();
});
document.addEventListener("keydown", (e) => {
  if (e.key === "Escape" && !lightbox.hidden) closeLightbox();
});

// ---------- 4. Reservation form validation ----------
const form = document.getElementById("reserveForm");
const successMsg = document.getElementById("successMsg");

// Block past dates in the date picker
const dateInput = document.getElementById("date");
const today = new Date();
const todayStr = new Date(today.getTime() - today.getTimezoneOffset() * 60000)
  .toISOString()
  .split("T")[0];
dateInput.min = todayStr;

// Each validator returns an error message, or "" if the value is fine
const validators = {
  name: (v) => {
    if (!v.trim()) return "Please enter your name.";
    if (!/^[A-Za-z][A-Za-z .'-]{1,49}$/.test(v.trim())) return "Use letters only, 2 to 50 characters.";
    return "";
  },
  email: (v) => {
    if (!v.trim()) return "Please enter your email.";
    if (!/^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/.test(v.trim())) return "Enter a valid email like name@example.com.";
    return "";
  },
  phone: (v) => {
    if (!v) return "Please enter your phone number.";
    if (!/^[6-9]\d{9}$/.test(v)) return "Enter a 10-digit mobile number starting with 6 to 9.";
    return "";
  },
  date: (v) => {
    if (!v) return "Please choose a date.";
    if (v < todayStr) return "Choose today or a future date.";
    return "";
  },
  time: (v) => {
    if (!v) return "Please choose a time.";
    if (v < "11:00" || v > "22:30") return "We take bookings from 11:00 am to 10:30 pm.";
    // If booking for today, the time must be in the future
    if (dateInput.value === todayStr) {
      const now = String(today.getHours()).padStart(2, "0") + ":" + String(today.getMinutes()).padStart(2, "0");
      if (v <= now) return "Choose a time later than now.";
    }
    return "";
  },
  guests: (v) => {
    if (!v) return "Enter the number of guests.";
    const n = Number(v);
    if (!Number.isInteger(n) || n < 1 || n > 10) return "We seat 1 to 10 guests online.";
    return "";
  },
};

function checkField(id) {
  const input = document.getElementById(id);
  const error = validators[id](input.value);
  document.getElementById(id + "Error").textContent = error;
  input.classList.toggle("invalid", Boolean(error));
  input.classList.toggle("valid", !error);
  if (error) {
    // restart the shake animation
    input.style.animation = "none";
    input.offsetHeight;
    input.style.animation = "";
  }
  return !error;
}

// Validate each field when the user leaves it
Object.keys(validators).forEach((id) => {
  const input = document.getElementById(id);
  input.addEventListener("blur", () => checkField(id));
  input.addEventListener("input", () => {
    if (input.classList.contains("invalid")) checkField(id);
  });
});
// Phone: allow digits only
document.getElementById("phone").addEventListener("input", (e) => {
  e.target.value = e.target.value.replace(/\D/g, "");
});

form.addEventListener("submit", (e) => {
  e.preventDefault();
  successMsg.hidden = true;

  // Check every field (don't stop at the first error)
  const results = Object.keys(validators).map(checkField);
  if (results.includes(false)) {
    form.querySelector(".invalid").focus();
    return;
  }

  const name = document.getElementById("name").value.trim().split(" ")[0];
  const guests = document.getElementById("guests").value;
  const when = new Date(dateInput.value + "T00:00").toLocaleDateString("en-IN", { day: "numeric", month: "short" });
  successMsg.textContent = `Thank you, ${name}! Table for ${guests} on ${when} at ${document.getElementById("time").value} is requested. We will call to confirm.`;
  successMsg.hidden = false;

  form.reset();
  form.querySelectorAll("input").forEach((i) => i.classList.remove("valid", "invalid"));
});
