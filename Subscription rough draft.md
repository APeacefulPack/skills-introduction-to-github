import React, { useState } from "react";

export default function SubscriptionSavingsCalculator() {
  const [dogs, setDogs] = useState(1);
  const [daycareDays, setDaycareDays] = useState(0);
  const [boardingNights, setBoardingNights] = useState(0);
  const [holidayNights, setHolidayNights] = useState(0);
  const [baths, setBaths] = useState(0);
  const [transportTrips, setTransportTrips] = useState(0);
  const [subscriptionType, setSubscriptionType] = useState("none");

  // Pricing Variables
  const daycareRate = 42;
  const boardingRate = 60;
  const holidayRate = 70;
  const bathPrice = 30;
  const transportFee = 10;

  // Subscription Pricing
  const basicSubPrice = 50;
  const standardSubPrice = 100 + (dogs > 1 ? (dogs - 1) * 25 : 0);
  const premiumSubPrice = 135 + (dogs > 1 ? (dogs - 1) * 30 : 0);

  // Subscription Discounts
  const basicDaycareRate = 35.70; // 15% off
  const standardDaycareRate = 38;
  const premiumDaycareRate = 38;
  const standardBoardingRate = 55;
  const premiumBoardingRate = 55;
  const standardHolidayRate = 60;
  const premiumHolidayRate = 55;
  const standardBathPrice = 18; // 40% off first 2 baths
  const premiumBathPrice = 0; // 2 free baths
  const basicBathPrice = 25; // $5 off

  // Transportation Discounts
  const standardTransportTrips = transportTrips > 4 ? (transportTrips - 4) * transportFee : 0;
  const premiumTransportTrips = transportTrips > 8 ? (transportTrips - 8) * transportFee : 0;

  // Calculate Costs
  const nonSubTotal =
    daycareDays * daycareRate +
    boardingNights * boardingRate +
    holidayNights * holidayRate +
    baths * bathPrice +
    transportTrips * transportFee;

  const basicTotal =
    basicSubPrice +
    daycareDays * basicDaycareRate +
    baths * basicBathPrice;

  const standardTotal =
    standardSubPrice +
    daycareDays * standardDaycareRate +
    boardingNights * standardBoardingRate +
    holidayNights * standardHolidayRate +
    (baths > 2 ? (2 * standardBathPrice) + ((baths - 2) * (bathPrice * 0.5)) : baths * standardBathPrice) +
    standardTransportTrips;

  const premiumTotal =
    premiumSubPrice +
    daycareDays * premiumDaycareRate +
    boardingNights * premiumBoardingRate +
    holidayNights * premiumHolidayRate +
    premiumTransportTrips; // No cost for first 2 baths, 50% off extras

  return (
    <div className="max-w-xl mx-auto p-6 bg-white shadow-xl rounded-lg border border-gray-200">
      <h2 className="text-3xl font-bold text-purple-900 mb-4 text-center">
        Subscription Savings Calculator
      </h2>

      <p className="text-gray-600 text-center mb-6">
        Find out how much you can save!
      </p>

      <div className="space-y-4">
        {[
          { label: "🐶 Number of Dogs:", value: dogs, setValue: setDogs },
          { label: "🌞 Monthly Daycare Days:", value: daycareDays, setValue: setDaycareDays },
          { label: "🏡 Regular Boarding Nights:", value: boardingNights, setValue: setBoardingNights },
          { label: "🎄 Holiday Boarding Nights:", value: holidayNights, setValue: setHolidayNights },
          { label: "🛁 Baths per Month:", value: baths, setValue: setBaths },
          { label: "🚗 Transportation Trips:", value: transportTrips, setValue: setTransportTrips },
        ].map((item, index) => (
          <label key={index} className="block text-gray-700">
            {item.label}
            <input
              type="number"
              min="0"
              value={item.value}
              onChange={(e) => item.setValue(Number(e.target.value))}
              className="border border-gray-300 rounded-lg p-2 w-full text-lg focus:ring-2 focus:ring-purple-600 focus:border-transparent"
            />
          </label>
        ))}

        <label className="block text-gray-700">
          📦 Subscription Type:
          <select
            value={subscriptionType}
            onChange={(e) => setSubscriptionType(e.target.value)}
            className="border border-gray-300 rounded-lg p-2 w-full text-lg focus:ring-2 focus:ring-purple-600 focus:border-transparent"
          >
            <option value="none">No Subscription</option>
            <option value="basic">Basic - $50/month</option>
            <option value="standard">Standard - $100/month</option>
            <option value="premium">Premium - $135/month</option>
          </select>
        </label>
      </div>

      <button className="mt-6 px-6 py-3 bg-purple-700 hover:bg-purple-900 text-white rounded-lg w-full transition-all">
        Calculate Savings
      </button>

      <div className="mt-6 p-4 bg-gray-100 rounded-lg shadow-inner">
        <h3 className="text-xl font-semibold text-purple-900">Your Monthly Costs</h3>

        <p className="text-gray-800">
          ❌ <strong>Without Subscription:</strong> 
          <span className="font-bold text-red-600"> ${nonSubTotal.toFixed(2)}</span>
        </p>

        {subscriptionType === "basic" && (
          <p className="text-gray-800">
            ✅ <strong>Basic Subscription:</strong> 
            <span className="font-bold text-green-700"> ${basicTotal.toFixed(2)}</span>
            <span className="text-green-600"> (Saves: ${nonSubTotal - basicTotal})</span>
          </p>
        )}

        {subscriptionType === "standard" && (
          <p className="text-gray-800">
            ✅ <strong>Standard Subscription:</strong> 
            <span className="font-bold text-green-700"> ${standardTotal.toFixed(2)}</span>
            <span className="text-green-600"> (Saves: ${nonSubTotal - standardTotal})</span>
          </p>
        )}

        {subscriptionType === "premium" && (
          <p className="text-gray-800">
            🔥 <strong>Premium Subscription:</strong> 
            <span className="font-bold text-blue-700"> ${premiumTotal.toFixed(2)}</span>
            <span className="text-green-600"> (Saves: ${nonSubTotal - premiumTotal})</span>
          </p>
        )}
      </div>
    </div>
  );
}