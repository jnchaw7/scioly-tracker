/* SciOly treasury portal · budget-workflow-v6 */
var SPREADSHEET_ID = "1HOCsBAzMKc-7w6KQpVWWOk8Bs3eo9Jda8xcIwtVNUhw";

/*
 * ============================================================
 * CONFIGURATION
 * ============================================================
 */

var HIGH_VALUE_THRESHOLD = 50;

var HIGH_VALUE_ALERT_EMAIL = "100034572@mvla.net";

var GOOGLE_CLIENT_ID =
  "483790698157-mk3l3ugqgvh4uv10q3bng2g9j8j3vp8q.apps.googleusercontent.com";

var USER_PROFILE_SHEET_NAME = "_UserProfiles";

var PORTAL_ACCESS_LOG_SHEET_NAME = "_PortalAccessLog";

var EXTERNAL_APPROVAL_SHEET_NAME = "_ExternalEmailApprovals";

var EVENT_BUDGET_SHEET_NAME = "_EventBudgets";

var BUDGET_APPROVAL_SHEET_NAME = "_BudgetApprovalRequests";

var SCHOOL_EMAIL_DOMAIN = "mvla.net";

var MAX_PROFILE_NAME_LENGTH = 60;

// Version 3 is the Next-only highlighted guided tour. Increasing this
// makes everyone who completed the older click-required tour see the
// improved guide once, while preserving their saved display name.
var CURRENT_ONBOARDING_VERSION = 3;

var SUPER_ADMIN_EMAILS = [
  "100034572@mvla.net",
  "jn.chaw7@gmail.com"
];

var EXECUTIVE_EMAILS = [
  "advisor-email@mvla.net",
  "leader-executive@mvla.net",
  "100034572@mvla.net",
  "100032279@mvla.net",
  "100032037@mvla.net",
  "100033176@mvla.net",
  "100033492@mvla.net",
  "100033205@mvla.net"
];

var PORTAL_MAINTENANCE_PROPERTY =
  "PORTAL_MAINTENANCE_MODE";


/*
 * ============================================================
 * SPREADSHEET COLUMN MAP
 * ============================================================
 *
 * Apps Script uses zero-based indexes.
 *
 * C = 2  -> Name
 * D = 3  -> Item
 * E = 4  -> Vendor Link
 * F = 5  -> Price
 * H = 7  -> Quantity
 * J = 9  -> Email
 * K = 10 -> Event
 * L = 11 -> Student ID
 * O = 14 -> Signatures / Progress
 * S = 18 -> Actual Status
 */


/*
 * ============================================================
 * POST API
 * ============================================================
 */

function doPost(e) {

  try {

    var data =
      JSON.parse(
        e.postData.contents
      );


    // ========================================================
    // GOOGLE SIGN-IN
    // ========================================================

    if (data.token) {

      var verifyUrl =
        "https://oauth2.googleapis.com/tokeninfo?id_token=" +
        encodeURIComponent(data.token);


      var response =
        UrlFetchApp.fetch(
          verifyUrl,
          {
            muteHttpExceptions: true
          }
        );


      var payload =
        JSON.parse(
          response.getContentText()
        );


      var emailIsVerified =
        payload.email_verified === true ||
        payload.email_verified === "true";


      var issuerIsGoogle =
        payload.iss === "accounts.google.com" ||
        payload.iss === "https://accounts.google.com";


      if (
        !payload.email ||
        !emailIsVerified ||
        !issuerIsGoogle ||
        payload.aud !== GOOGLE_CLIENT_ID
      ) {

        return jsonResponse({

          type: "error",

          message:
            "Invalid Google Authentication Token."

        });

      }


      var userEmail =
        payload.email
          .toString()
          .trim()
          .toLowerCase();


      var isSuperAdmin =
        isSuperAdminEmail_(
          userEmail
        );


      var isExecutive =
        isExecutiveEmail_(
          userEmail
        );


      var maintenanceMode =
        getPortalMaintenanceMode_();


      // ======================================================
      // OWNER-ONLY ADMIN ACTIONS
      // ======================================================

      if (
        data.action ===
        "setMaintenanceMode"
      ) {

        if (!isSuperAdmin) {

          return adminDeniedResponse_();

        }


        maintenanceMode =
          setPortalMaintenanceMode_(
            data.enabled === true
          );


        return jsonResponse({

          type:
            "adminAction",

          action:
            "setMaintenanceMode",

          isSuperAdmin:
            true,

          maintenanceMode:
            maintenanceMode

        });

      }


      if (
        data.action ===
        "resetUserProfiles"
      ) {

        if (!isSuperAdmin) {

          return adminDeniedResponse_();

        }


        if (
          data.confirmation !==
          "RESET ONBOARDING"
        ) {

          throw new Error(
            'Type "RESET ONBOARDING" exactly to continue.'
          );

        }


        var profileReset =
          resetUserProfiles_();


        return jsonResponse({

          type:
            "adminAction",

          action:
            "resetUserProfiles",

          isSuperAdmin:
            true,

          maintenanceMode:
            maintenanceMode,

          result:
            profileReset

        });

      }


      // ======================================================
      // MAINTENANCE GATE
      //
      // Super admins remain able to sign in and turn the portal
      // back on. Every other account is stopped before any data
      // or profile action is processed.
      // ======================================================

      if (
        maintenanceMode &&
        !isSuperAdmin
      ) {

        return jsonResponse({

          type:
            "maintenance",

          message:
            "The SciOly ordering portal is temporarily unavailable while maintenance is in progress. Please try again later."

        });

      }


      if (
        data.action ===
        "login"
      ) {

        recordPortalAccess_(
          userEmail
        );

      }


      // ======================================================
      // EXTERNAL EMAIL GATE
      //
      // mvla.net accounts are trusted automatically. Any other
      // address found in Master remains pending until an
      // executive approves it in the portal. Executive and owner
      // accounts always retain access to the approval queue.
      // ======================================================

      if (
        !isExecutive &&
        !isSchoolEmail_(
          userEmail
        )
      ) {

        var externalStatus =
          getExternalApprovalStatus_(
            userEmail
          );


        if (
          externalStatus !==
          "APPROVED"
        ) {

          return jsonResponse({

            type:
              "waitlisted",

            email:
              userEmail,

            status:
              externalStatus,

            message:
              externalStatus === "REJECTED"
                ? "This external email has not been approved for the SciOly ordering portal. Please use your school account or contact a team executive."
                : "This external email is waiting for executive approval. Students should sign in with their mvla.net school account."

          });

        }

      }


      // ======================================================
      // PROFILE SAVE
      //
      // The name is always attached to the email extracted
      // from the verified Google ID token. The browser cannot
      // choose a different email address to edit.
      // ======================================================

      if (
        data.action ===
        "saveProfile"
      ) {

        var savedProfile =
          saveUserProfile_(
            userEmail,
            data.name
          );


        return jsonResponse({

          type:
            "profileSaved",

          email:
            userEmail,

          isSuperAdmin:
            isSuperAdmin,

          maintenanceMode:
            maintenanceMode,

          profile:
            savedProfile

        });

      }


      if (
        data.action ===
        "completeOnboarding"
      ) {

        var completedProfile =
          completeUserOnboarding_(
            userEmail
          );


        return jsonResponse({

          type:
            "onboardingComplete",

          email:
            userEmail,

          isSuperAdmin:
            isSuperAdmin,

          maintenanceMode:
            maintenanceMode,

          profile:
            completedProfile

        });

      }


      var userProfile =
        getUserProfile_(
          userEmail
        );


      // ======================================================
      // PERSONAL BUDGET-OVERAGE REQUEST
      //
      // The browser supplies only the event. The backend rebuilds
      // the signed-in user's orders and budget before accepting it.
      // ======================================================

      if (
        data.action ===
        "requestBudgetApproval"
      ) {

        return jsonResponse({

          type:
            "budgetApprovalRequested",

          email:
            userEmail,

          data:
            requestBudgetApproval_(
              userEmail,
              data.eventName
            )

        });

      }


      // ======================================================
      // EXECUTIVE
      // ======================================================

      if (isExecutive) {

        // ----------------------------------------------------
        // EVENT BUDGETS AND OVERAGE APPROVALS
        // ----------------------------------------------------

        if (
          data.action ===
          "budgetCenter"
        ) {

          return jsonResponse({

            type:
              "budgetCenter",

            email:
              userEmail,

            data:
              fetchBudgetCenter_()

          });

        }


        if (
          data.action ===
          "setBudgetApproval"
        ) {

          return jsonResponse({

            type:
              "budgetCenter",

            email:
              userEmail,

            data:
              setBudgetApproval_(
                data.requestId,
                data.status,
                userEmail
              )

          });

        }


        if (
          data.action ===
          "setBudgetLimit"
        ) {

          return jsonResponse({

            type:
              "budgetCenter",

            email:
              userEmail,

            data:
              saveBudgetRule_(
                data.rule || {},
                userEmail
              )

          });

        }

        // ----------------------------------------------------
        // EXTERNAL EMAIL APPROVAL QUEUE
        // ----------------------------------------------------

        if (
          data.action ===
          "externalApprovals"
        ) {

          return jsonResponse({

            type:
              "externalApprovals",

            email:
              userEmail,

            data:
              fetchExternalApprovals_()

          });

        }


        if (
          data.action ===
          "setExternalApproval"
        ) {

          return jsonResponse({

            type:
              "externalApprovals",

            email:
              userEmail,

            data:
              setExternalApproval_(
                data.email,
                data.status,
                userEmail
              )

          });

        }

        // ----------------------------------------------------
        // TEAM STATISTICS
        // ----------------------------------------------------

        if (
          data.action ===
          "teamStatistics"
        ) {

          var statistics =
            fetchTeamStatistics();


          statistics.portalAccess =
            fetchPortalAccessSummary_();


          return jsonResponse({

            type:
              "teamStatistics",

            email:
              userEmail,

            isSuperAdmin:
              isSuperAdmin,

            maintenanceMode:
              maintenanceMode,

            data:
              statistics

          });

        }


        // ----------------------------------------------------
        // EXECUTIVE PERSONAL DASHBOARD
        // ----------------------------------------------------

        var execData =
          fetchRowsForEmail(
            userEmail
          );


        var externalApprovalSummary =
          fetchExternalApprovals_();


        var budgetApprovalSummary =
          fetchBudgetApprovalSummary_();


        return jsonResponse({

          type:
            "exec",

          email:
            userEmail,

          isSuperAdmin:
            isSuperAdmin,

          maintenanceMode:
            maintenanceMode,

          profile:
            userProfile,

          externalApprovalSummary: {

            pendingCount:
              externalApprovalSummary.pendingCount,

            approvedCount:
              externalApprovalSummary.approvedCount,

            rejectedCount:
              externalApprovalSummary.rejectedCount

          },

          budgetApprovalSummary:
            budgetApprovalSummary,

          data:
            execData

        });

      }


      // ======================================================
      // REJECT EXECUTIVE-ONLY ACTIONS FOR EVERYONE ELSE
      // ======================================================

      if (
        data.action === "teamStatistics" ||
        data.action === "externalApprovals" ||
        data.action === "setExternalApproval" ||
        data.action === "budgetCenter" ||
        data.action === "setBudgetApproval" ||
        data.action === "setBudgetLimit"
      ) {

        return jsonResponse({

          type:
            "error",

          message:
            "This account is not authorized to view team insights."

        });

      }


      // ======================================================
      // NORMAL STUDENT
      // ======================================================

      var studentData =
        fetchRowsForEmail(
          userEmail
        );


      return jsonResponse({

        type:
          "student",

        email:
          userEmail,

        isSuperAdmin:
          false,

        maintenanceMode:
          maintenanceMode,

        profile:
          userProfile,

        data:
          studentData

      });

    }


    return jsonResponse({

      type:
        "error",

      message:
        "Invalid API payload request."

    });


  } catch (error) {

    return jsonResponse({

      type:
        "error",

      message:
        error.message

    });

  }

}


/*
 * ============================================================
 * GET
 * ============================================================
 */

function doGet(e) {

  return jsonResponse({

    type:
      "error",

    message:
      "Please use POST protocol for authentication."

  });

}


/*
 * ============================================================
 * JSON RESPONSE
 * ============================================================
 */

function jsonResponse(data) {

  return ContentService
    .createTextOutput(
      JSON.stringify(data)
    )
    .setMimeType(
      ContentService.MimeType.JSON
    );

}


/*
 * ============================================================
 * OWNER / MAINTENANCE CONTROLS
 * ============================================================
 */

function isSuperAdminEmail_(email) {

  return SUPER_ADMIN_EMAILS.indexOf(
    normalizeEmail(
      email
    )
  ) !== -1;

}


function isExecutiveEmail_(email) {

  var normalizedEmail =
    normalizeEmail(
      email
    );


  return (
    isSuperAdminEmail_(
      normalizedEmail
    ) ||
    EXECUTIVE_EMAILS.indexOf(
      normalizedEmail
    ) !== -1
  );

}


function getPortalMaintenanceMode_() {

  return PropertiesService
    .getScriptProperties()
    .getProperty(
      PORTAL_MAINTENANCE_PROPERTY
    ) === "true";

}


function setPortalMaintenanceMode_(enabled) {

  var properties =
    PropertiesService
      .getScriptProperties();


  if (enabled) {

    properties.setProperty(
      PORTAL_MAINTENANCE_PROPERTY,
      "true"
    );

  } else {

    properties.deleteProperty(
      PORTAL_MAINTENANCE_PROPERTY
    );

  }


  return enabled;

}


/*
 * Emergency editor controls.
 *
 * If the frontend is being repaired and the Admin page cannot
 * load, run either function directly from the Apps Script
 * editor using the account that owns the script project.
 */

function turnPortalOffManually() {

  setPortalMaintenanceMode_(
    true
  );

}


function turnPortalBackOnManually() {

  setPortalMaintenanceMode_(
    false
  );

}


function adminDeniedResponse_() {

  return jsonResponse({

    type:
      "error",

    message:
      "This account is not authorized to use owner controls."

  });

}


function clearPortalSheetRows_(sheetName) {

  var ss =
    SpreadsheetApp.openById(
      SPREADSHEET_ID
    );


  var sheet =
    ss.getSheetByName(
      sheetName
    );


  if (!sheet) {

    return 0;

  }


  var lastRow =
    sheet.getLastRow();


  if (lastRow < 2) {

    return 0;

  }


  var rowsCleared =
    lastRow - 1;


  sheet
    .getRange(
      2,
      1,
      rowsCleared,
      sheet.getMaxColumns()
    )
    .clearContent();


  return rowsCleared;

}


function resetUserProfiles_() {

  var lock =
    LockService.getScriptLock();


  lock.waitLock(
    10000
  );


  try {

    return {

      profilesCleared:
        clearPortalSheetRows_(
          USER_PROFILE_SHEET_NAME
        )

    };

  } finally {

    lock.releaseLock();

  }

}


/*
 * ============================================================
 * FETCH ORDERS FOR ONE EMAIL
 * ============================================================
 *
 * IMPORTANT:
 *
 * Column J is the internal email identifier.
 * Column C is only the display name.
 */

/*
 * ============================================================
 * PERSON IDENTITY HELPERS
 * ============================================================
 *
 * Matching priority:
 *
 * 1. Same normalized email = definitely the same person.
 * 2. Otherwise, same normalized Student ID = same person.
 *    The Student ID is normalized to its last 5 digits so that
 *    values such as 1000XXXXX and 100XXXXX still match.
 * 3. Name is used as a confirmation / consistency check only.
 *    Name by itself NEVER merges two people.
 */

function normalizeEmail(value) {

  return value
    ? value
        .toString()
        .trim()
        .toLowerCase()
    : "";

}


/*
 * ============================================================
 * EXTERNAL EMAIL APPROVALS
 * ============================================================
 *
 * School accounts are accepted automatically. Every other email
 * found in column J of Master is added to a hidden approval queue.
 * The frontend can request or change this queue only after doPost
 * has verified that the signed-in Google account is an executive.
 *
 * _ExternalEmailApprovals columns:
 * A = external email
 * B = PENDING / APPROVED / REJECTED
 * C = first seen
 * D = last decision time
 * E = decision made by
 */

function isSchoolEmail_(email) {

  var normalizedEmail =
    normalizeEmail(
      email
    );


  var atIndex =
    normalizedEmail.lastIndexOf(
      "@"
    );


  if (
    atIndex <= 0 ||
    atIndex === normalizedEmail.length - 1
  ) {

    return false;

  }


  return (
    normalizedEmail.slice(
      atIndex + 1
    ) === SCHOOL_EMAIL_DOMAIN
  );

}


function ensureExternalApprovalSheet_() {

  var ss =
    SpreadsheetApp.openById(
      SPREADSHEET_ID
    );


  var sheet =
    ss.getSheetByName(
      EXTERNAL_APPROVAL_SHEET_NAME
    );


  if (sheet) {

    sheet
      .getRange(
        1,
        1,
        1,
        5
      )
      .setValues([[
        "Email",
        "Status",
        "First Seen",
        "Updated At",
        "Updated By"
      ]])
      .setFontWeight(
        "bold"
      );


    return sheet;

  }


  var lock =
    LockService.getScriptLock();


  lock.waitLock(
    10000
  );


  try {

    sheet =
      ss.getSheetByName(
        EXTERNAL_APPROVAL_SHEET_NAME
      );


    if (!sheet) {

      sheet =
        ss.insertSheet(
          EXTERNAL_APPROVAL_SHEET_NAME
        );


      sheet
        .getRange(
          1,
          1,
          1,
          5
        )
        .setValues([[
          "Email",
          "Status",
          "First Seen",
          "Updated At",
          "Updated By"
        ]])
        .setFontWeight(
          "bold"
        );


      sheet.setFrozenRows(
        1
      );


      sheet.hideSheet();

    }


    return sheet;

  } finally {

    lock.releaseLock();

  }

}


function getExternalApprovalStatus_(email) {

  var normalizedEmail =
    normalizeEmail(
      email
    );


  if (
    isSchoolEmail_(
      normalizedEmail
    ) ||
    isExecutiveEmail_(
      normalizedEmail
    )
  ) {

    return "APPROVED";

  }


  var sheet =
    ensureExternalApprovalSheet_();


  if (sheet.getLastRow() < 2) {

    return "PENDING";

  }


  var values =
    sheet
      .getRange(
        2,
        1,
        sheet.getLastRow() - 1,
        2
      )
      .getDisplayValues();


  for (
    var i = 0;
    i < values.length;
    i++
  ) {

    if (
      normalizeEmail(
        values[i][0]
      ) === normalizedEmail
    ) {

      var status =
        values[i][1]
          ? values[i][1]
              .toString()
              .trim()
              .toUpperCase()
          : "PENDING";


      return (
        status === "APPROVED" ||
        status === "REJECTED"
      )
        ? status
        : "PENDING";

    }

  }


  return "PENDING";

}


function getApprovedExternalEmailMap_() {

  var approved = {};


  var sheet =
    ensureExternalApprovalSheet_();


  if (sheet.getLastRow() < 2) {

    return approved;

  }


  var values =
    sheet
      .getRange(
        2,
        1,
        sheet.getLastRow() - 1,
        2
      )
      .getDisplayValues();


  values.forEach(
    function(row) {

      var email =
        normalizeEmail(
          row[0]
        );


      if (
        email &&
        String(
          row[1] || ""
        )
          .trim()
          .toUpperCase() === "APPROVED"
      ) {

        approved[email] = true;

      }

    }
  );


  return approved;

}


function isOrderEmailAllowed_(email, approvedExternalEmails) {

  var normalizedEmail =
    normalizeEmail(
      email
    );


  if (!normalizedEmail) {

    return false;

  }


  return (
    isSchoolEmail_(
      normalizedEmail
    ) ||
    isExecutiveEmail_(
      normalizedEmail
    ) ||
    approvedExternalEmails[
      normalizedEmail
    ] === true
  );

}


function collectExternalOrderCandidates_() {

  var ss =
    SpreadsheetApp.openById(
      SPREADSHEET_ID
    );


  var master =
    ss.getSheetByName(
      "Master"
    );


  if (!master) {

    throw new Error(
      'Sheet "Master" was not found.'
    );

  }


  var data =
    master
      .getDataRange()
      .getValues();


  var candidates = {};


  for (
    var i = 1;
    i < data.length;
    i++
  ) {

    var row =
      data[i];


    var email =
      normalizeEmail(
        row[9]
      );


    if (
      !email ||
      isSchoolEmail_(
        email
      ) ||
      isExecutiveEmail_(
        email
      )
    ) {

      continue;

    }


    if (!candidates[email]) {

      candidates[email] = {

        email:
          email,

        names:
          {},

        events:
          {},

        orderCount:
          0,

        totalCost:
          0,

        items:
          []

      };

    }


    var name =
      row[2]
        ? row[2]
            .toString()
            .trim()
        : "Unknown";


    var itemName =
      row[3]
        ? row[3]
            .toString()
            .trim()
        : "Unnamed item";


    var eventName =
      row[10]
        ? row[10]
            .toString()
            .trim()
        : "Unassigned event";


    var quantity =
      parseFloat(
        row[7]
      );


    if (
      isNaN(
        quantity
      ) ||
      quantity <= 0
    ) {

      quantity = 1;

    }


    var total =
      parsePrice(
        row[5]
      ) * quantity;


    candidates[email].names[name] = true;
    candidates[email].events[eventName] = true;
    candidates[email].orderCount++;
    candidates[email].totalCost += total;


    candidates[email].items.push({

      rowNum:
        i + 1,

      name:
        name,

      itemName:
        itemName,

      eventName:
        eventName,

      quantity:
        quantity,

      totalCost:
        total

    });

  }


  return candidates;

}


function approvalDateToIso_(value) {

  var date =
    value instanceof Date
      ? value
      : new Date(
          value
        );


  return isNaN(
    date.getTime()
  )
    ? ""
    : date.toISOString();

}


function fetchExternalApprovals_() {

  var candidates =
    collectExternalOrderCandidates_();


  var sheet =
    ensureExternalApprovalSheet_();


  var lock =
    LockService.getScriptLock();


  lock.waitLock(
    10000
  );


  try {

    var existingEmails = {};


    if (sheet.getLastRow() >= 2) {

      sheet
        .getRange(
          2,
          1,
          sheet.getLastRow() - 1,
          1
        )
        .getDisplayValues()
        .forEach(
          function(row) {

            var email =
              normalizeEmail(
                row[0]
              );


            if (email) {

              existingEmails[email] = true;

            }

          }
        );

    }


    var newRows = [];


    Object.keys(
      candidates
    )
      .forEach(
        function(email) {

          if (!existingEmails[email]) {

            newRows.push([
              email,
              "PENDING",
              new Date(),
              "",
              ""
            ]);

          }

        }
      );


    if (newRows.length) {

      sheet
        .getRange(
          sheet.getLastRow() + 1,
          1,
          newRows.length,
          5
        )
        .setValues(
          newRows
        );

    }

  } finally {

    lock.releaseLock();

  }


  var decisions = {};


  if (sheet.getLastRow() >= 2) {

    sheet
      .getRange(
        2,
        1,
        sheet.getLastRow() - 1,
        5
      )
      .getValues()
      .forEach(
        function(row) {

          var email =
            normalizeEmail(
              row[0]
            );


          if (!email) {

            return;

          }


          var status =
            String(
              row[1] || "PENDING"
            )
              .trim()
              .toUpperCase();


          if (
            status !== "APPROVED" &&
            status !== "REJECTED"
          ) {

            status = "PENDING";

          }


          decisions[email] = {

            email:
              email,

            status:
              status,

            firstSeen:
              approvalDateToIso_(
                row[2]
              ),

            updatedAt:
              approvalDateToIso_(
                row[3]
              ),

            updatedBy:
              normalizeEmail(
                row[4]
              )

          };

        }
      );

  }


  var allEmails = {};


  Object.keys(candidates).forEach(
    function(email) {

      allEmails[email] = true;

    }
  );


  Object.keys(decisions).forEach(
    function(email) {

      allEmails[email] = true;

    }
  );


  var entries =
    Object.keys(
      allEmails
    )
      .map(
        function(email) {

          var candidate =
            candidates[email] || {
              names: {},
              events: {},
              orderCount: 0,
              totalCost: 0,
              items: []
            };


          var decision =
            decisions[email] || {
              email: email,
              status: "PENDING",
              firstSeen: "",
              updatedAt: "",
              updatedBy: ""
            };


          return {

            email:
              email,

            status:
              decision.status,

            firstSeen:
              decision.firstSeen,

            updatedAt:
              decision.updatedAt,

            updatedBy:
              decision.updatedBy,

            names:
              Object.keys(
                candidate.names
              ),

            events:
              Object.keys(
                candidate.events
              ),

            orderCount:
              candidate.orderCount,

            totalCost:
              candidate.totalCost,

            items:
              candidate.items.slice(
                0,
                30
              )

          };

        }
      );


  var statusOrder = {
    PENDING: 0,
    APPROVED: 1,
    REJECTED: 2
  };


  entries.sort(
    function(a, b) {

      return (
        statusOrder[a.status] -
        statusOrder[b.status]
      ) ||
      a.email.localeCompare(
        b.email
      );

    }
  );


  return {

    pendingCount:
      entries.filter(function(entry) {
        return entry.status === "PENDING";
      }).length,

    approvedCount:
      entries.filter(function(entry) {
        return entry.status === "APPROVED";
      }).length,

    rejectedCount:
      entries.filter(function(entry) {
        return entry.status === "REJECTED";
      }).length,

    entries:
      entries

  };

}


function setExternalApproval_(email, status, actorEmail) {

  var normalizedEmail =
    normalizeEmail(
      email
    );


  var normalizedStatus =
    String(
      status || ""
    )
      .trim()
      .toUpperCase();


  if (
    !normalizedEmail ||
    isSchoolEmail_(
      normalizedEmail
    ) ||
    isExecutiveEmail_(
      normalizedEmail
    )
  ) {

    throw new Error(
      "Only non-school form emails can be changed here."
    );

  }


  if (
    [
      "PENDING",
      "APPROVED",
      "REJECTED"
    ].indexOf(
      normalizedStatus
    ) === -1
  ) {

    throw new Error(
      "Invalid external-email approval status."
    );

  }


  fetchExternalApprovals_();


  var sheet =
    ensureExternalApprovalSheet_();


  var lock =
    LockService.getScriptLock();


  lock.waitLock(
    10000
  );


  try {

    var lastRow =
      sheet.getLastRow();


    var values =
      lastRow >= 2
        ? sheet
            .getRange(
              2,
              1,
              lastRow - 1,
              1
            )
            .getDisplayValues()
        : [];


    var targetRow = 0;


    for (
      var i = 0;
      i < values.length;
      i++
    ) {

      if (
        normalizeEmail(
          values[i][0]
        ) === normalizedEmail
      ) {

        targetRow =
          i + 2;

        break;

      }

    }


    if (!targetRow) {

      throw new Error(
        "That email is not currently present in the ordering sheet."
      );

    }


    sheet
      .getRange(
        targetRow,
        2,
        1,
        4
      )
      .setValues([[
        normalizedStatus,
        sheet
          .getRange(
            targetRow,
            3
          )
          .getValue() || new Date(),
        new Date(),
        normalizeEmail(
          actorEmail
        )
      ]]);


  } finally {

    lock.releaseLock();

  }


  return fetchExternalApprovals_();

}


/*
 * Optional manual sync for testing from the Apps Script editor.
 * The executive approval page also performs this sync automatically.
 */
function syncExternalEmailWaitlist() {

  return fetchExternalApprovals_();

}


/*
 * ============================================================
 * USER DISPLAY-NAME PROFILES
 * ============================================================
 *
 * Profiles are stored in a hidden sheet:
 *
 * A = verified Google email
 * B = chosen display name
 * C = last updated time
 * D = orientation completed
 *
 * The email is never accepted from the browser. doPost() passes
 * only the email extracted from the verified Google ID token.
 */

function ensureUserProfilesSheet_() {

  var ss =
    SpreadsheetApp.openById(
      SPREADSHEET_ID
    );


  var sheet =
    ss.getSheetByName(
      USER_PROFILE_SHEET_NAME
    );


  if (sheet) {

    ensureUserProfileHeaders_(
      sheet
    );

    return sheet;

  }


  var lock =
    LockService.getScriptLock();


  lock.waitLock(
    10000
  );


  try {

    sheet =
      ss.getSheetByName(
        USER_PROFILE_SHEET_NAME
      );


    if (!sheet) {

      sheet =
        ss.insertSheet(
          USER_PROFILE_SHEET_NAME
        );


      sheet
        .getRange(
          1,
          1,
          1,
          4
        )
        .setValues([[
          "Email",
          "Display Name",
          "Updated At",
          "Onboarding Version"
        ]]);


      sheet
        .getRange(
          1,
          1,
          1,
          4
        )
        .setFontWeight(
          "bold"
        );


      sheet.setFrozenRows(
        1
      );


      sheet.hideSheet();

    }


    return sheet;

  } finally {

    lock.releaseLock();

  }

}


function ensureUserProfileHeaders_(sheet) {

  sheet
    .getRange(
      1,
      1,
      1,
      4
    )
    .setValues([[
      "Email",
      "Display Name",
      "Updated At",
      "Onboarding Version"
    ]])
    .setFontWeight(
      "bold"
    );

}


function cleanProfileName_(value) {

  var name =
    value === null ||
    value === undefined

      ? ""

      : value
          .toString()
          .replace(
            /[\u0000-\u001F\u007F]/g,
            " "
          )
          .replace(
            /\s+/g,
            " "
          )
          .trim();


  if (!name) {

    throw new Error(
      "Please enter your name."
    );

  }


  if (
    name.length >
    MAX_PROFILE_NAME_LENGTH
  ) {

    throw new Error(
      "Your name must be " +
      MAX_PROFILE_NAME_LENGTH +
      " characters or fewer."
    );

  }


  return name;

}


function getUserProfile_(email) {

  var normalizedEmail =
    normalizeEmail(
      email
    );


  if (!normalizedEmail) {

    return {

      name:
        "",

      needsName:
        true,

      onboardingComplete:
        false,

      onboardingVersion:
        0,

      needsOnboarding:
        true

    };

  }


  var sheet =
    ensureUserProfilesSheet_();


  var lastRow =
    sheet.getLastRow();


  if (
    lastRow <
    2
  ) {

    return {

      name:
        "",

      needsName:
        true,

      onboardingComplete:
        false,

      onboardingVersion:
        0,

      needsOnboarding:
        true

    };

  }


  var values =
    sheet
      .getRange(
        2,
        1,
        lastRow - 1,
        4
      )
      .getDisplayValues();


  for (
    var i = 0;
    i < values.length;
    i++
  ) {

    if (
      normalizeEmail(
        values[i][0]
      ) ===
      normalizedEmail
    ) {

      var storedName =
        values[i][1]
          ? values[i][1]
              .toString()
              .trim()
          : "";


      var onboardingVersion =
        parseInt(
          values[i][3],
          10
        );


      if (
        isNaN(
          onboardingVersion
        )
      ) {

        onboardingVersion = 0;

      }


      var onboardingComplete =
        onboardingVersion >=
        CURRENT_ONBOARDING_VERSION;


      return {

        name:
          storedName,

        needsName:
          !storedName,

        onboardingComplete:
          onboardingComplete,

        onboardingVersion:
          onboardingVersion,

        needsOnboarding:
          !storedName ||
          !onboardingComplete

      };

    }

  }


  return {

    name:
      "",

    needsName:
      true,

    onboardingComplete:
      false,

    onboardingVersion:
      0,

    needsOnboarding:
      true

  };

}


function saveUserProfile_(email, value) {

  var normalizedEmail =
    normalizeEmail(
      email
    );


  if (!normalizedEmail) {

    throw new Error(
      "A verified Google email is required."
    );

  }


  var name =
    cleanProfileName_(
      value
    );


  var sheet =
    ensureUserProfilesSheet_();


  var lock =
    LockService.getScriptLock();


  lock.waitLock(
    10000
  );


  try {

    var lastRow =
      sheet.getLastRow();


    var targetRow =
      lastRow + 1;


    var isNewProfile =
      true;


    if (
      lastRow >=
      2
    ) {

      var emails =
        sheet
          .getRange(
            2,
            1,
            lastRow - 1,
            1
          )
          .getDisplayValues();


      for (
        var i = 0;
        i < emails.length;
        i++
      ) {

        if (
          normalizeEmail(
            emails[i][0]
          ) ===
          normalizedEmail
        ) {

          targetRow =
            i + 2;


          isNewProfile =
            false;

          break;

        }

      }

    }


    sheet
      .getRange(
        targetRow,
        1
      )
      .setNumberFormat(
        "@"
      )
      .setValue(
        normalizedEmail
      );


    sheet
      .getRange(
        targetRow,
        2
      )
      .setNumberFormat(
        "@"
      )
      .setValue(
        name
      );


    sheet
      .getRange(
        targetRow,
        3
      )
      .setValue(
        new Date()
      )
      .setNumberFormat(
        "yyyy-mm-dd hh:mm:ss"
      );


    if (isNewProfile) {

      sheet
        .getRange(
          targetRow,
          4
        )
        .setValue(
          0
        );

    }


    var onboardingVersion =
      Number(
        sheet
          .getRange(
            targetRow,
            4
          )
          .getValue() ||
          0
      );


    var onboardingComplete =
      onboardingVersion >=
      CURRENT_ONBOARDING_VERSION;


    return {

      name:
        name,

      needsName:
        false,

      onboardingComplete:
        onboardingComplete,

      onboardingVersion:
        onboardingVersion,

      needsOnboarding:
        !onboardingComplete

    };

  } finally {

    lock.releaseLock();

  }

}


function completeUserOnboarding_(email) {

  var normalizedEmail =
    normalizeEmail(
      email
    );


  var sheet =
    ensureUserProfilesSheet_();


  var lock =
    LockService.getScriptLock();


  lock.waitLock(
    10000
  );


  try {

    var lastRow =
      sheet.getLastRow();


    if (lastRow < 2) {

      throw new Error(
        "Please save your name before completing orientation."
      );

    }


    var values =
      sheet
        .getRange(
          2,
          1,
          lastRow - 1,
          2
        )
        .getDisplayValues();


    for (
      var i = 0;
      i < values.length;
      i++
    ) {

      if (
        normalizeEmail(
          values[i][0]
        ) ===
        normalizedEmail
      ) {

        var name =
          values[i][1]
            ? values[i][1]
                .toString()
                .trim()
            : "";


        if (!name) {

          throw new Error(
            "Please save your name before completing orientation."
          );

        }


        var targetRow =
          i + 2;


        sheet
          .getRange(
            targetRow,
            4
          )
          .setValue(
            CURRENT_ONBOARDING_VERSION
          );


        sheet
          .getRange(
            targetRow,
            3
          )
          .setValue(
            new Date()
          )
          .setNumberFormat(
            "yyyy-mm-dd hh:mm:ss"
          );


        return {

          name:
            name,

          needsName:
            false,

          onboardingComplete:
            true,

          onboardingVersion:
            CURRENT_ONBOARDING_VERSION,

          needsOnboarding:
            false

        };

      }

    }


    throw new Error(
      "Please save your name before completing orientation."
    );

  } finally {

    lock.releaseLock();

  }

}


/*
 * ============================================================
 * PORTAL ACCESS LOG
 * ============================================================
 *
 * One row is appended only when the frontend sends action=login.
 * Refreshes, searches, profile saves, and statistics requests do
 * not create duplicate visit rows.
 *
 * A = login time
 * B = verified Google email
 */

function ensurePortalAccessLogSheet_() {

  var ss =
    SpreadsheetApp.openById(
      SPREADSHEET_ID
    );


  var sheet =
    ss.getSheetByName(
      PORTAL_ACCESS_LOG_SHEET_NAME
    );


  if (sheet) {

    return sheet;

  }


  var lock =
    LockService.getScriptLock();


  lock.waitLock(
    10000
  );


  try {

    sheet =
      ss.getSheetByName(
        PORTAL_ACCESS_LOG_SHEET_NAME
      );


    if (!sheet) {

      sheet =
        ss.insertSheet(
          PORTAL_ACCESS_LOG_SHEET_NAME
        );


      sheet
        .getRange(
          1,
          1,
          1,
          2
        )
        .setValues([[
          "Login Time",
          "Verified Email"
        ]])
        .setFontWeight(
          "bold"
        );


      sheet.setFrozenRows(
        1
      );


      sheet.hideSheet();

    }


    return sheet;

  } finally {

    lock.releaseLock();

  }

}


function recordPortalAccess_(email) {

  var normalizedEmail =
    normalizeEmail(
      email
    );


  if (!normalizedEmail) {

    return;

  }


  var sheet =
    ensurePortalAccessLogSheet_();


  var lock =
    LockService.getScriptLock();


  lock.waitLock(
    10000
  );


  try {

    var row =
      sheet.getLastRow() + 1;


    sheet
      .getRange(
        row,
        1
      )
      .setValue(
        new Date()
      )
      .setNumberFormat(
        "yyyy-mm-dd hh:mm:ss"
      );


    sheet
      .getRange(
        row,
        2
      )
      .setNumberFormat(
        "@"
      )
      .setValue(
        normalizedEmail
      );

  } finally {

    lock.releaseLock();

  }

}


function fetchPortalAccessSummary_() {

  var sheet =
    ensurePortalAccessLogSheet_();


  var entries = [];


  if (
    sheet.getLastRow() >=
    2
  ) {

    var values =
      sheet
        .getRange(
          2,
          1,
          sheet.getLastRow() - 1,
          2
        )
        .getValues();


    values.forEach(
      function(row) {

        var email =
          normalizeEmail(
            row[1]
          );


        var timestamp =
          row[0] instanceof Date
            ? row[0]
            : new Date(
                row[0]
              );


        if (
          email &&
          !isNaN(
            timestamp.getTime()
          )
        ) {

          entries.push({

            email:
              email,

            timestamp:
              timestamp.toISOString()

          });

        }

      }
    );

  }


  var lastLoginByEmail = {};


  entries.forEach(
    function(entry) {

      lastLoginByEmail[
        entry.email
      ] = entry.timestamp;

    }
  );


  var ss =
    SpreadsheetApp.openById(
      SPREADSHEET_ID
    );


  var master =
    ss.getSheetByName(
      "Master"
    );


  var rosterByEmail = {};


  var approvedExternalEmails =
    getApprovedExternalEmailMap_();


  if (master) {

    var masterValues =
      master
        .getDataRange()
        .getValues();


    for (
      var i = 1;
      i < masterValues.length;
      i++
    ) {

      var rosterEmail =
        normalizeEmail(
          masterValues[i][9]
        );


      if (
        rosterEmail &&
        isOrderEmailAllowed_(
          rosterEmail,
          approvedExternalEmails
        ) &&
        !rosterByEmail[rosterEmail]
      ) {

        rosterByEmail[rosterEmail] =
          masterValues[i][2]
            ? masterValues[i][2]
                .toString()
                .trim()
            : "Unknown";

      }

    }

  }


  var neverLoggedIn =
    Object.keys(
      rosterByEmail
    )
    .filter(
      function(email) {

        return !lastLoginByEmail[email];

      }
    )
    .map(
      function(email) {

        return {

          name:
            rosterByEmail[email],

          email:
            email

        };

      }
    )
    .sort(
      function(a, b) {

        return a.name.localeCompare(
          b.name
        );

      }
    );


  return {

    totalLogins:
      entries.length,

    uniqueVisitors:
      Object.keys(
        lastLoginByEmail
      ).length,

    recentLogins:
      entries
        .slice()
        .reverse()
        .slice(
          0,
          100
        ),

    neverLoggedIn:
      neverLoggedIn

  };

}


function normalizePersonName(value) {

  return value
    ? value
        .toString()
        .trim()
        .toLowerCase()
        .replace(/\s+/g, " ")
    : "";

}


function normalizeStudentId(value) {

  if (
    value === null ||
    value === undefined
  ) {

    return "";

  }


  var digits =
    value
      .toString()
      .replace(/\D/g, "");


  /*
   * Require at least 5 digits.  A shorter value is too weak to
   * use as an identity signal and could cause false matches.
   */
  if (digits.length < 5) {

    return "";

  }


  return digits.slice(-5);

}


function buildPersonIdentityGroups(data) {

  var parent = [];
  var rank = [];


  for (
    var i = 1;
    i < data.length;
    i++
  ) {

    parent[i] = i;
    rank[i] = 0;

  }


  function find(rowIndex) {

    if (
      parent[rowIndex] !== rowIndex
    ) {

      parent[rowIndex] =
        find(
          parent[rowIndex]
        );

    }


    return parent[rowIndex];

  }


  function union(a, b) {

    var rootA =
      find(a);

    var rootB =
      find(b);


    if (rootA === rootB) {
      return;
    }


    if (
      rank[rootA] < rank[rootB]
    ) {

      parent[rootA] = rootB;

    } else if (
      rank[rootA] > rank[rootB]
    ) {

      parent[rootB] = rootA;

    } else {

      parent[rootB] = rootA;
      rank[rootA]++;

    }

  }


  var firstRowForEmail = {};
  var firstRowForStudentId = {};


  for (
    var rowIndex = 1;
    rowIndex < data.length;
    rowIndex++
  ) {

    var row =
      data[rowIndex];


    var email =
      normalizeEmail(
        row[9]
      );


    var name =
      normalizePersonName(
        row[2]
      );


    // Column L = Student ID
    var studentId =
      normalizeStudentId(
        row[11]
      );


    // --------------------------------------------------------
    // SAME EMAIL = DEFINITE MATCH
    // --------------------------------------------------------
    if (email) {

      if (
        firstRowForEmail[email] !== undefined
      ) {

        union(
          rowIndex,
          firstRowForEmail[email]
        );

      } else {

        firstRowForEmail[email] =
          rowIndex;

      }

    }


    // --------------------------------------------------------
    // SAME STUDENT ID = STRONG MATCH
    // --------------------------------------------------------
    if (studentId) {

      if (
        firstRowForStudentId[studentId] !== undefined
      ) {

        var previousRowIndex =
          firstRowForStudentId[studentId];


        var previousName =
          normalizePersonName(
            data[previousRowIndex][2]
          );


        /*
         * Name is confirmation only.  If the ID matches but the
         * names differ, the ID still wins, but log it so a typo can
         * be found in the sheet.
         */
        if (
          name &&
          previousName &&
          name !== previousName
        ) {

          Logger.log(
            "Student ID suffix [" +
            studentId +
            "] matched across different names: [" +
            previousName +
            "] vs [" +
            name +
            "]"
          );

        }


        union(
          rowIndex,
          previousRowIndex
        );

      } else {

        firstRowForStudentId[studentId] =
          rowIndex;

      }

    }


    /*
     * IMPORTANT: there is deliberately NO name-only matching here.
     */

  }


  /*
   * Compress every row to its final group after all email/ID links
   * have been processed.  This makes transitive cases work too:
   * email A -> ID X -> email B -> corrected ID Y -> email C.
   */
  for (
    var compressIndex = 1;
    compressIndex < data.length;
    compressIndex++
  ) {

    find(compressIndex);

  }


  return {

    find:
      find

  };

}


/*
 * ============================================================
 * PER-PERSON / PER-EVENT BUDGETS
 * ============================================================
 *
 * _EventBudgets columns:
 * A Rule ID | B Scope Type | C Scope Value | D Event
 * E Budget | F Updated At | G Updated By
 *
 * Scope Type can be DEFAULT, EMAIL, STUDENT_ID, or PERSON_NAME.
 * A DEFAULT rule applies the event limit separately to every person.
 * More-specific rules override the default for that person only.
 *
 * _BudgetApprovalRequests columns:
 * A Request ID | B Email | C Person | D Student ID | E Event
 * F Budget | G Spend Requested | H Triggering Master Row
 * I Triggering Item | J Triggering Order Total | K Status
 * L Requested At | M Decided At | N Decided By | O Note
 */

function ensureEventBudgetSheet_() {

  var ss = SpreadsheetApp.openById(SPREADSHEET_ID);
  var sheet = ss.getSheetByName(EVENT_BUDGET_SHEET_NAME);

  if (!sheet) {

    sheet = ss.insertSheet(EVENT_BUDGET_SHEET_NAME);
    sheet.getRange(1, 1, 1, 7).setValues([[
      "Rule ID",
      "Scope Type",
      "Scope Value",
      "Event",
      "Budget",
      "Updated At",
      "Updated By"
    ]]);
    sheet.setFrozenRows(1);

  }

  if (sheet.getLastRow() < 2) {

    sheet.getRange(2, 1, 1, 7).setValues([[
      "DEFAULT-ELECTRIC-VEHICLE",
      "DEFAULT",
      "*",
      "Electric Vehicle",
      50,
      new Date(),
      "SYSTEM"
    ]]);

  }

  if (!sheet.isSheetHidden()) {

    sheet.hideSheet();

  }

  return sheet;

}


function ensureBudgetApprovalSheet_() {

  var ss = SpreadsheetApp.openById(SPREADSHEET_ID);
  var sheet = ss.getSheetByName(BUDGET_APPROVAL_SHEET_NAME);

  if (!sheet) {

    sheet = ss.insertSheet(BUDGET_APPROVAL_SHEET_NAME);
    sheet.getRange(1, 1, 1, 15).setValues([[
      "Request ID",
      "Email",
      "Person",
      "Student ID",
      "Event",
      "Budget",
      "Spend Requested",
      "Triggering Master Row",
      "Triggering Item",
      "Triggering Order Total",
      "Status",
      "Requested At",
      "Decided At",
      "Decided By",
      "Note"
    ]]);
    sheet.setFrozenRows(1);

  }

  if (!sheet.isSheetHidden()) {

    sheet.hideSheet();

  }

  return sheet;

}


function normalizeBudgetEvent_(value) {

  return String(value || "")
    .replace(/\s+/g, " ")
    .trim()
    .toLowerCase();

}


function readBudgetRules_() {

  var sheet = ensureEventBudgetSheet_();

  if (sheet.getLastRow() < 2) {

    return [];

  }

  return sheet
    .getRange(2, 1, sheet.getLastRow() - 1, 7)
    .getValues()
    .map(function(row, index) {

      var scopeType = String(row[1] || "DEFAULT")
        .trim()
        .toUpperCase();

      if (["DEFAULT", "EMAIL", "STUDENT_ID", "PERSON_NAME"].indexOf(scopeType) === -1) {

        scopeType = "DEFAULT";

      }

      return {
        rowNum: index + 2,
        ruleId: String(row[0] || "RULE-" + (index + 2)).trim(),
        scopeType: scopeType,
        scopeValue: String(row[2] || "*").trim(),
        eventName: String(row[3] || "").replace(/\s+/g, " ").trim(),
        budget: parsePrice(row[4]),
        updatedAt: approvalDateToIso_(row[5]),
        updatedBy: normalizeEmail(row[6]) || String(row[6] || "")
      };

    })
    .filter(function(rule) {

      return Boolean(rule.eventName) && rule.budget > 0;

    });

}


function findApplicableBudgetRule_(email, orders, eventName, rules) {

  var normalizedEmail = normalizeEmail(email);
  var eventKey = normalizeBudgetEvent_(eventName);
  var emails = {};
  var studentIds = {};
  var personNames = {};

  if (normalizedEmail) emails[normalizedEmail] = true;

  orders.forEach(function(order) {

    var orderEmail = normalizeEmail(order.email);
    var studentId = normalizeStudentId(order.studentIdSuffix);
    var personName = normalizePersonName(order.personName);
    if (orderEmail) emails[orderEmail] = true;
    if (studentId) studentIds[studentId] = true;
    if (personName) personNames[personName] = true;

  });

  var bestRule = null;
  var bestScore = -1;

  rules.forEach(function(rule) {

    if (normalizeBudgetEvent_(rule.eventName) !== eventKey) return;

    var score = -1;

    if (rule.scopeType === "EMAIL" && emails[normalizeEmail(rule.scopeValue)]) score = 400;
    if (rule.scopeType === "STUDENT_ID" && studentIds[normalizeStudentId(rule.scopeValue)]) score = 300;
    if (rule.scopeType === "PERSON_NAME" && personNames[normalizePersonName(rule.scopeValue)]) score = 200;
    if (rule.scopeType === "DEFAULT") score = 100;

    if (score >= bestScore && score >= 0) {

      bestRule = rule;
      bestScore = score;

    }

  });

  return bestRule;

}


function readBudgetApprovalRequests_() {

  var sheet = ensureBudgetApprovalSheet_();

  if (sheet.getLastRow() < 2) {

    return [];

  }

  return sheet
    .getRange(2, 1, sheet.getLastRow() - 1, 15)
    .getValues()
    .map(function(row, index) {

      var status = String(row[10] || "PENDING").trim().toUpperCase();

      if (["PENDING", "APPROVED", "REJECTED"].indexOf(status) === -1) {

        status = "PENDING";

      }

      return {
        rowNum: index + 2,
        requestId: String(row[0] || "").trim(),
        email: normalizeEmail(row[1]),
        personName: String(row[2] || "Unknown").trim(),
        studentId: normalizeStudentId(row[3]),
        eventName: String(row[4] || "Unassigned Event").trim(),
        budget: parsePrice(row[5]),
        spendRequested: parsePrice(row[6]),
        triggeringRow: Number(row[7] || 0),
        triggeringItem: String(row[8] || "Unnamed item").trim(),
        triggeringOrderTotal: parsePrice(row[9]),
        status: status,
        requestedAt: approvalDateToIso_(row[11]),
        decidedAt: approvalDateToIso_(row[12]),
        decidedBy: normalizeEmail(row[13]),
        note: String(row[14] || "").trim()
      };

    })
    .filter(function(request) {

      return Boolean(request.requestId && request.email);

    });

}


function latestBudgetRequest_(requests, email, eventName) {

  var emailKey = normalizeEmail(email);
  var eventKey = normalizeBudgetEvent_(eventName);
  var matches = requests.filter(function(request) {

    return request.email === emailKey && normalizeBudgetEvent_(request.eventName) === eventKey;

  });

  matches.sort(function(a, b) {

    return b.rowNum - a.rowNum;

  });

  return matches.length ? matches[0] : null;

}


function isCancelledBudgetOrder_(order) {

  var stage = String(order.stage || "").trim().toUpperCase();
  var status = String(order.orderStatus || "").trim().toLowerCase();
  return stage === "CANCELLED" || status.indexOf("cancel") !== -1;

}


function buildPersonalBudgetSummary_(email, userRows) {

  var rules = readBudgetRules_();
  var requests = readBudgetApprovalRequests_();
  var eventGroups = {};

  userRows.forEach(function(order) {

    if (isCancelledBudgetOrder_(order)) return;

    var eventName = String(order.eventName || "Unassigned Event").replace(/\s+/g, " ").trim();
    var eventKey = normalizeBudgetEvent_(eventName);

    if (!eventGroups[eventKey]) {

      eventGroups[eventKey] = {
        eventName: eventName,
        orders: []
      };

    }

    eventGroups[eventKey].orders.push(order);

  });

  return Object.keys(eventGroups)
    .map(function(eventKey) {

      var group = eventGroups[eventKey];
      var orders = group.orders.slice().sort(function(a, b) {

        return Number(a.rowNum || 0) - Number(b.rowNum || 0);

      });
      var rule = findApplicableBudgetRule_(email, orders, group.eventName, rules);

      if (!rule) return null;

      var spend = orders.reduce(function(total, order) {

        return total + Number(order.numericPrice || 0);

      }, 0);
      var budget = Number(rule.budget || 0);
      var latest = latestBudgetRequest_(requests, email, group.eventName);
      var status = spend > budget ? "NEEDS_APPROVAL" : "WITHIN_BUDGET";
      var threshold = budget;

      if (spend > budget && latest) {

        if (latest.status === "APPROVED" && spend <= latest.spendRequested + 0.005) {

          status = "APPROVED";

        } else if (latest.status === "PENDING" && spend <= latest.spendRequested + 0.005) {

          status = "PENDING";

        } else if (latest.status === "REJECTED" && spend <= latest.spendRequested + 0.005) {

          status = "REJECTED";

        }

        if (
          latest.status === "APPROVED" &&
          spend > latest.spendRequested + 0.005
        ) {

          threshold = Math.max(threshold, Number(latest.spendRequested || 0));

        }

      }

      var running = 0;
      var triggeringOrder = null;

      orders.forEach(function(order) {

        running += Number(order.numericPrice || 0);
        if (!triggeringOrder && running > threshold + 0.005) triggeringOrder = order;

      });

      if (spend > budget && !triggeringOrder) triggeringOrder = orders[orders.length - 1] || null;

      var firstOrder = orders[0] || {};

      return {
        eventName: group.eventName,
        personName: String(firstOrder.personName || "Unknown"),
        studentId: normalizeStudentId(firstOrder.studentIdSuffix),
        budget: budget,
        spent: spend,
        remaining: Math.max(0, budget - spend),
        overflowAmount: Math.max(0, spend - budget),
        percent: budget > 0 ? Math.min(100, Math.max(0, spend / budget * 100)) : 0,
        status: status,
        isOverBudget: spend > budget + 0.005,
        requiresApproval: status === "NEEDS_APPROVAL" || status === "REJECTED",
        requestId: latest ? latest.requestId : "",
        requestStatus: latest ? latest.status : "",
        approvedThrough: latest && latest.status === "APPROVED" ? latest.spendRequested : 0,
        triggeringOrder: triggeringOrder ? {
          rowNum: Number(triggeringOrder.rowNum || 0),
          itemName: String(triggeringOrder.itemName || "Unnamed item"),
          orderTotal: Number(triggeringOrder.numericPrice || 0)
        } : null,
        rule: {
          ruleId: rule.ruleId,
          scopeType: rule.scopeType,
          scopeValue: rule.scopeValue
        }
      };

    })
    .filter(function(summary) {

      return Boolean(summary);

    })
    .sort(function(a, b) {

      if (a.isOverBudget !== b.isOverBudget) return a.isOverBudget ? -1 : 1;
      return a.eventName.localeCompare(b.eventName);

    });

}


function requestBudgetApproval_(email, eventName) {

  var normalizedEvent = normalizeBudgetEvent_(eventName);

  if (!normalizedEvent) throw new Error("Choose an event before requesting approval.");

  var dashboard = fetchRowsForEmail(email);
  var summary = (dashboard.eventBudgets || []).filter(function(entry) {

    return normalizeBudgetEvent_(entry.eventName) === normalizedEvent;

  })[0];

  if (!summary) throw new Error("No budget has been assigned to this event yet.");
  if (!summary.isOverBudget) throw new Error("This event is still within its assigned budget.");
  if (summary.status === "APPROVED") throw new Error("This amount already has executive approval.");
  if (summary.status === "PENDING") throw new Error("An approval request for this amount is already waiting.");

  var lock = LockService.getScriptLock();
  lock.waitLock(10000);

  try {

    var latest = latestBudgetRequest_(readBudgetApprovalRequests_(), email, summary.eventName);

    if (latest && latest.status === "PENDING" && latest.spendRequested >= summary.spent - 0.005) {

      throw new Error("An approval request for this amount is already waiting.");

    }

    var trigger = summary.triggeringOrder || {};
    ensureBudgetApprovalSheet_()
      .appendRow([
        Utilities.getUuid(),
        normalizeEmail(email),
        summary.personName,
        summary.studentId,
        summary.eventName,
        summary.budget,
        summary.spent,
        Number(trigger.rowNum || 0),
        String(trigger.itemName || "Unnamed item"),
        Number(trigger.orderTotal || 0),
        "PENDING",
        new Date(),
        "",
        "",
        "Student requested an exception for the current event total."
      ]);

  } finally {

    lock.releaseLock();

  }

  return fetchRowsForEmail(email);

}


function fetchBudgetApprovalSummary_() {

  var requests = readBudgetApprovalRequests_();

  return {
    pendingCount: requests.filter(function(request) { return request.status === "PENDING"; }).length,
    approvedCount: requests.filter(function(request) { return request.status === "APPROVED"; }).length,
    rejectedCount: requests.filter(function(request) { return request.status === "REJECTED"; }).length
  };

}


function fetchBudgetCenter_() {

  var requests = readBudgetApprovalRequests_();
  var rules = readBudgetRules_();
  var statusOrder = { PENDING: 0, APPROVED: 1, REJECTED: 2 };

  requests.sort(function(a, b) {

    return (statusOrder[a.status] - statusOrder[b.status]) || b.rowNum - a.rowNum;

  });

  return {
    pendingCount: requests.filter(function(request) { return request.status === "PENDING"; }).length,
    approvedCount: requests.filter(function(request) { return request.status === "APPROVED"; }).length,
    rejectedCount: requests.filter(function(request) { return request.status === "REJECTED"; }).length,
    requests: requests,
    rules: rules
  };

}


function setBudgetApproval_(requestId, status, actorEmail) {

  var normalizedId = String(requestId || "").trim();
  var normalizedStatus = String(status || "").trim().toUpperCase();

  if (!normalizedId) throw new Error("The budget request ID is missing.");
  if (["APPROVED", "REJECTED"].indexOf(normalizedStatus) === -1) {

    throw new Error("A budget request can only be approved or rejected.");

  }

  var sheet = ensureBudgetApprovalSheet_();
  var lock = LockService.getScriptLock();
  lock.waitLock(10000);

  try {

    var values = sheet.getLastRow() >= 2
      ? sheet.getRange(2, 1, sheet.getLastRow() - 1, 1).getDisplayValues()
      : [];
    var targetRow = 0;

    for (var i = 0; i < values.length; i++) {

      if (String(values[i][0] || "").trim() === normalizedId) {

        targetRow = i + 2;
        break;

      }

    }

    if (!targetRow) throw new Error("That budget request could not be found.");

    sheet.getRange(targetRow, 11).setValue(normalizedStatus);
    sheet.getRange(targetRow, 13).setValue(new Date());
    sheet.getRange(targetRow, 14).setValue(normalizeEmail(actorEmail));

  } finally {

    lock.releaseLock();

  }

  return fetchBudgetCenter_();

}


function saveBudgetRule_(input, actorEmail) {

  input = input || {};

  var ruleId = String(input.ruleId || "").trim();
  var scopeType = String(input.scopeType || "DEFAULT").trim().toUpperCase();
  var scopeValue = String(input.scopeValue || "").replace(/\s+/g, " ").trim();
  var eventName = String(input.eventName || "").replace(/\s+/g, " ").trim();
  var budget = parsePrice(input.budget);

  if (["DEFAULT", "EMAIL", "STUDENT_ID", "PERSON_NAME"].indexOf(scopeType) === -1) {

    throw new Error("Choose a valid budget scope.");

  }

  if (!eventName) throw new Error("Enter the Science Olympiad event name.");
  if (!(budget > 0) || budget > 1000000) throw new Error("Enter a budget greater than $0 and no more than $1,000,000.");

  if (scopeType === "DEFAULT") scopeValue = "*";
  if (scopeType === "EMAIL") {

    scopeValue = normalizeEmail(scopeValue);
    if (!scopeValue || scopeValue.indexOf("@") === -1) throw new Error("Enter a valid email for this person-specific rule.");

  }
  if (scopeType === "STUDENT_ID") {

    scopeValue = normalizeStudentId(scopeValue);
    if (!scopeValue) throw new Error("Enter a student ID for this person-specific rule.");

  }
  if (scopeType === "PERSON_NAME" && !scopeValue) throw new Error("Enter the person's name for this rule.");

  var sheet = ensureEventBudgetSheet_();
  var lock = LockService.getScriptLock();
  lock.waitLock(10000);

  try {

    var values = sheet.getLastRow() >= 2
      ? sheet.getRange(2, 1, sheet.getLastRow() - 1, 5).getValues()
      : [];
    var targetRow = 0;

    for (var i = 0; i < values.length; i++) {

      var existingId = String(values[i][0] || "").trim();
      var sameNaturalKey =
        String(values[i][1] || "DEFAULT").trim().toUpperCase() === scopeType &&
        String(values[i][2] || "").trim().toLowerCase() === scopeValue.toLowerCase() &&
        normalizeBudgetEvent_(values[i][3]) === normalizeBudgetEvent_(eventName);

      if ((ruleId && existingId === ruleId) || (!ruleId && sameNaturalKey)) {

        targetRow = i + 2;
        if (!ruleId) ruleId = existingId;
        break;

      }

    }

    if (!ruleId) ruleId = "BUDGET-" + Utilities.getUuid();

    var row = [
      ruleId,
      scopeType,
      scopeValue,
      eventName,
      budget,
      new Date(),
      normalizeEmail(actorEmail)
    ];

    if (targetRow) sheet.getRange(targetRow, 1, 1, 7).setValues([row]);
    else sheet.appendRow(row);

  } finally {

    lock.releaseLock();

  }

  return fetchBudgetCenter_();

}


function fetchRowsForEmail(email) {

  var ss =
    SpreadsheetApp.openById(
      SPREADSHEET_ID
    );


  var sheet =
    ss.getSheetByName(
      "Master"
    );


  if (!sheet) {

    throw new Error(
      'Sheet "Master" was not found.'
    );

  }


  // ========================================================
  // READ MASTER ONCE
  // ========================================================

  var data =
    sheet
      .getDataRange()
      .getValues();


  /*
   * Build the timing model ONCE per request.
   *
   * Do not rebuild it for every order card.
   */
  var timingModel =
    buildDeliveryTimingModel_();


  var userRows = [];


  var normalizedEmail =
    normalizeEmail(
      email
    );


  var approvedExternalEmails =
    getApprovedExternalEmailMap_();


  var identityGroups =
    buildPersonIdentityGroups(
      data
    );


  // ========================================================
  // FIND THIS USER'S IDENTITY GROUP
  // ========================================================

  var targetGroups = {};


  for (
    var seedIndex = 1;
    seedIndex < data.length;
    seedIndex++
  ) {

    var seedEmail =
      normalizeEmail(
        data[seedIndex][9]
      );


    if (
      seedEmail ===
      normalizedEmail &&
      isOrderEmailAllowed_(
        seedEmail,
        approvedExternalEmails
      )
    ) {

      targetGroups[
        identityGroups.find(
          seedIndex
        )
      ] = true;

    }

  }


  // ========================================================
  // PROCESS USER'S ORDERS
  // ========================================================

  for (
    var i = 1;
    i < data.length;
    i++
  ) {

    /*
     * row EXISTS from this point onward.
     */
    var row =
      data[i];


    var rowEmail =
      normalizeEmail(
        row[9]
      );


    if (
      !isOrderEmailAllowed_(
        rowEmail,
        approvedExternalEmails
      )
    ) {

      continue;

    }


    var rowGroup =
      identityGroups.find(
        i
      );


    if (
      !targetGroups[
        rowGroup
      ]
    ) {

      continue;

    }


    // ======================================================
    // COLUMN C = PERSON NAME
    // ======================================================

    var personName =
      row[2]
        ? row[2]
            .toString()
            .trim()
        : "Unknown";


    // ======================================================
    // COLUMN L = STUDENT ID
    // ======================================================

    var studentIdSuffix =
      normalizeStudentId(
        row[11]
      );


    // ======================================================
    // COLUMN D = ITEM
    // ======================================================

    var itemName =
      row[3]
        ? row[3]
            .toString()
            .trim()
        : "Unnamed Item";


    // ======================================================
    // COLUMN E = VENDOR LINK
    // ======================================================

    var link =
      row[4]
        ? row[4]
            .toString()
            .trim()
        : "";


    // ======================================================
    // COLUMN F = UNIT PRICE
    // ======================================================

    var unitPrice =
      parsePrice(
        row[5]
      );


    // ======================================================
    // COLUMN H = QUANTITY
    // ======================================================

    var qty =
      row[7]
        ? row[7]
            .toString()
            .trim()
        : "1";


    var numericQuantity =
      parseFloat(
        qty
      );


    if (
      isNaN(
        numericQuantity
      ) ||
      numericQuantity <= 0
    ) {

      numericQuantity = 1;

    }


    // ======================================================
    // REAL ORDER COST
    //
    // Column F = price PER ITEM
    // Column H = quantity
    //
    // Total = F × H
    // ======================================================

    var orderTotal =
      unitPrice *
      numericQuantity;


    var priceText =
      "$" +
      orderTotal.toFixed(
        2
      );


    // ======================================================
    // COLUMN K = EVENT
    // ======================================================

    var eventName =
      row[10]
        ? row[10]
            .toString()
            .trim()
        : "Unassigned Event";


    // ======================================================
    // COLUMN O = SIGNATURE / APPROVAL PROGRESS
    // ======================================================

    var signatures =
      row[14]
        ? row[14]
            .toString()
            .trim()
        : "Pending";


    // ======================================================
    // COLUMN S = ACTUAL SHIPPING STATUS
    // ======================================================

    var orderStatus =
      row[18]
        ? row[18]
            .toString()
            .trim()
        : "Pending";


    // ======================================================
    // BUILD THIS ORDER'S TRACKING ID
    //
    // IMPORTANT:
    // This MUST be inside the loop because it uses this row.
    // ======================================================

    var orderKey =
      buildTrackingOrderKey_(
        row,
        i + 1
      );


    // ======================================================
    // SMART DELIVERY ESTIMATE
    // ======================================================

    var velocity =
      calculateVelocityDelivery(

        signatures,

        orderStatus,

        link,

        orderKey,

        timingModel

      );


    // ======================================================
    // RETURN ORDER TO FRONTEND
    // ======================================================

    userRows.push({

      rowNum:
        i + 1,


      email:
        rowEmail,


      personName:
        personName,


      studentIdSuffix:
        studentIdSuffix,


      itemName:
        itemName,


      link:
        link,


      /*
       * "price" is now the TOTAL line-item price.
       */
      price:
        priceText,


      /*
       * Helpful if you ever want to display:
       *
       * $8.50 each × 4 = $34.00
       */
      unitPrice:
        unitPrice,


      numericQuantity:
        numericQuantity,


      /*
       * Budgets/statistics should use this value.
       */
      numericPrice:
        orderTotal,


      qty:
        qty,


      eventName:
        eventName,


      signatures:
        signatures,


      orderStatus:
        orderStatus,


      step:
        velocity.step,


      deliveryText:
        velocity.text,


      deliveryClass:
        velocity.styleClass,


      isStuck:
        velocity.isStuck,


      isDelivered:
        velocity.isDelivered,


      stage:
        velocity.stage,


      vendor:
        velocity.vendor,


      estimatedDeliveryAt:
        velocity.estimatedDeliveryAt,


      nextSignatureAt:
        velocity.nextSignatureAt,


      estimateConfidence:
        velocity.confidence,

      estimateConfidenceLabel:
        velocity.confidenceLabel,

      estimateConfidenceDetail:
        velocity.confidenceDetail

    });

  }


  // ========================================================
  // PERSONAL TOTAL
  // ========================================================

  var totalCost =
    userRows.reduce(

      function(
        total,
        item
      ) {

        return (
          total +
          item.numericPrice
        );

      },

      0

    );


  var eventBudgets =
    buildPersonalBudgetSummary_(
      normalizedEmail,
      userRows
    );
  

  return {
    
    email:
      normalizedEmail,

    totalCost:
      totalCost,

    eventBudgets:
      eventBudgets,

    items:
      userRows

  };

}


/*
 * ============================================================
 * PRICE PARSER
 * ============================================================
 */

function parsePrice(value) {

  if (
    value === null ||
    value === undefined
  ) {

    return 0;

  }


  var numeric =
    parseFloat(
      value
        .toString()
        .replace(
          /[^0-9.-]/g,
          ""
        )
    );


  return isNaN(numeric)
    ? 0
    : numeric;

}


/*
 * ============================================================
 * VELOCITY ESTIMATION
 * ============================================================
 *
 * Progress is based on Column O.
 */

/*
 * ============================================================
 * SMART DELIVERY / APPROVAL ESTIMATOR
 * ============================================================
 *
 * Uses:
 *
 * Column A  = original form timestamp, when available
 * Column D  = item
 * Column E  = vendor URL
 * Column J  = email
 * Column K  = event
 * Column L  = student ID
 * Column O  = signature / approval progress
 * Column S  = actual order status
 *
 * The estimator learns over time from a hidden history sheet.
 *
 * Approval timing:
 *   Historical transition timing + regression
 *
 * After signatures:
 *   Historical data if available
 *   fallback = 3 days until ordered
 *
 * Delivery:
 *   Amazon fallback = 5 days after ordering
 *   Other fallback  = 7 days after ordering
 *
 * Actual Column S status ALWAYS overrides estimates.
 */


/*
 * ============================================================
 * CONFIGURATION
 * ============================================================
 */

var TRACKING_HISTORY_SHEET =
  "_TrackingHistory";


var TRACKING_SCAN_MINUTES =
  15;


// ------------------------------------------------------------
// FALLBACKS
//
// These are only used while not enough historical data exists.
// As history grows, the estimator replaces these with learned
// values.
// ------------------------------------------------------------

var APPROVAL_FALLBACK_HOURS_PER_STEP =
  72;      // 3 days


var SIGNATURES_TO_ORDER_FALLBACK_HOURS =
  72;      // 3 days


var AMAZON_ORDER_TO_DELIVERED_FALLBACK_HOURS =
  120;     // 5 days


var OTHER_ORDER_TO_DELIVERED_FALLBACK_HOURS =
  168;     // 7 days


var AMAZON_SHIPPED_TO_DELIVERED_FALLBACK_HOURS =
  48;      // 2 days


var OTHER_SHIPPED_TO_DELIVERED_FALLBACK_HOURS =
  72;      // 3 days



/*
 * ============================================================
 * ONE-TIME SETUP
 *
 * Run setupTrackingEstimator() manually ONCE.
 *
 * It:
 *
 * 1. Creates the hidden _TrackingHistory sheet
 * 2. Records the current state
 * 3. Creates a 15-minute background scanner
 * ============================================================
 */

function setupTrackingEstimator() {

  ensureTrackingHistorySheet_();


  // Save current state as the initial baseline.

  recordTrackingChanges();


  /*
   * Remove an old copy of our tracking trigger first so running
   * this function twice does not create duplicate triggers.
   */

  var triggers =
    ScriptApp.getProjectTriggers();


  triggers.forEach(
    function(trigger) {

      if (
        trigger.getHandlerFunction() ===
        "recordTrackingChanges"
      ) {

        ScriptApp.deleteTrigger(
          trigger
        );

      }

    }
  );


  ScriptApp

    .newTrigger(
      "recordTrackingChanges"
    )

    .timeBased()

    .everyMinutes(
      TRACKING_SCAN_MINUTES
    )

    .create();


  Logger.log(

    "Tracking estimator installed. " +

    "Scanning every " +

    TRACKING_SCAN_MINUTES +

    " minutes."

  );

}



/*
 * ============================================================
 * CREATE / GET HISTORY SHEET
 * ============================================================
 */

function ensureTrackingHistorySheet_() {

  var ss =
    SpreadsheetApp.openById(
      SPREADSHEET_ID
    );


  var sheet =
    ss.getSheetByName(
      TRACKING_HISTORY_SHEET
    );


  if (!sheet) {

    sheet =
      ss.insertSheet(
        TRACKING_HISTORY_SHEET
      );


    sheet

      .getRange(
        1,
        1,
        1,
        13
      )

      .setValues([
        [

          "Observed At",
          "Order Key",
          "Master Row",
          "Form Timestamp",

          "Email",
          "Student ID",
          "Item",
          "Event",

          "Vendor",

          "Signature Text",
          "Approval Step",

          "Status Text",
          "Status Stage"

        ]
      ]);


    sheet.setFrozenRows(
      1
    );


    /*
     * Keep it out of the way.
     *
     * The data still exists and can be unhidden later if you
     * want to inspect it.
     */

    sheet.hideSheet();

  }


  return sheet;

}



/*
 * ============================================================
 * BACKGROUND CHANGE RECORDER
 * ============================================================
 *
 * This function runs every 15 minutes.
 *
 * It does NOT write a new history row every time.
 *
 * It only records when either:
 *
 * - Column O changes
 * - approval step changes
 * - Column S changes
 * - normalized shipping state changes
 *
 * Therefore the history stays reasonably compact.
 * ============================================================
 */

function recordTrackingChanges() {

  var ss =
    SpreadsheetApp.openById(
      SPREADSHEET_ID
    );


  var master =
    ss.getSheetByName(
      "Master"
    );


  if (!master) {

    throw new Error(
      'Sheet "Master" was not found.'
    );

  }


  var data =
    master

      .getDataRange()

      .getValues();


  var historySheet =
    ensureTrackingHistorySheet_();


  var history =
    readTrackingHistory_();


  /*
   * Find the newest recorded state for every order.
   */

  var latestByOrder =
    {};


  history.forEach(
    function(entry) {

      var old =
        latestByOrder[
          entry.orderKey
        ];


      if (
        !old ||
        entry.observedAt.getTime() >
        old.observedAt.getTime()
      ) {

        latestByOrder[
          entry.orderKey
        ] =
          entry;

      }

    }
  );


  /*
   * IMPORTANT:
   *
   * Every changed row in this scan receives EXACTLY the same
   * timestamp.
   *
   * This is useful because rows from one batch that all advance
   * simultaneously are recognizable as one synchronized event.
   */

  var now =
    new Date();


  var newRows =
    [];


  for (
    var i = 1;
    i < data.length;
    i++
  ) {

    var row =
      data[i];


    var email =
      normalizeEmail(
        row[9]
      );


    var itemName =
      row[3]

        ? row[3]
            .toString()
            .trim()

        : "";


    /*
     * Ignore empty/non-order rows.
     */

    if (
      !email ||
      !itemName
    ) {

      continue;

    }


    var orderKey =
      buildTrackingOrderKey_(
        row,
        i + 1
      );


    var signatures =
      row[14]

        ? row[14]
            .toString()
            .trim()

        : "Pending";


    var statusText =
      row[18]

        ? row[18]
            .toString()
            .trim()

        : "Pending";


    var step =
      parseApprovalStep_(
        signatures
      );


    var statusStage =
      normalizeStatusStage_(
        statusText
      );


    var vendor =
      detectVendorType_(
        row[4]
      );


    var latest =
      latestByOrder[
        orderKey
      ];


    /*
     * Only write history when something meaningful changed.
     */

    var changed =

      !latest ||

      latest.signatureText !==
        signatures ||

      latest.approvalStep !==
        step ||

      latest.statusText !==
        statusText ||

      latest.statusStage !==
        statusStage;


    if (!changed) {

      continue;

    }


    /*
     * Column A is commonly the Google Form submission
     * timestamp.
     *
     * If it isn't a Date in your sheet, we simply preserve
     * whatever value is there.
     */

    var formTimestamp =

      row[0] instanceof Date

        ? row[0]

        : (
            row[0]

              ? row[0]
                  .toString()

              : ""
          );


    newRows.push([

      now,

      orderKey,

      i + 1,

      formTimestamp,

      email,

      normalizeStudentId(
        row[11]
      ),

      itemName,

      row[10]
        ? row[10]
            .toString()
            .trim()
        : "Unassigned Event",

      vendor,

      signatures,

      step,

      statusText,

      statusStage

    ]);

  }


  if (
    newRows.length >
    0
  ) {

    historySheet

      .getRange(

        historySheet.getLastRow() + 1,

        1,

        newRows.length,

        newRows[0].length

      )

      .setValues(
        newRows
      );

  }

}



/*
 * ============================================================
 * READ HISTORY
 * ============================================================
 */

function readTrackingHistory_() {

  var sheet =
    ensureTrackingHistorySheet_();


  var values =
    sheet

      .getDataRange()

      .getValues();


  var result =
    [];


  for (
    var i = 1;
    i < values.length;
    i++
  ) {

    var row =
      values[i];


    if (
      !(row[0] instanceof Date) ||
      !row[1]
    ) {

      continue;

    }


    result.push({

      observedAt:
        row[0],

      orderKey:
        String(
          row[1]
        ),

      masterRow:
        Number(
          row[2] ||
          0
        ),

      formTimestamp:
        row[3],

      email:
        normalizeEmail(
          row[4]
        ),

      studentId:
        row[5]
          ? String(
              row[5]
            )
          : "",

      itemName:
        row[6]
          ? String(
              row[6]
            )
          : "",

      eventName:
        row[7]
          ? String(
              row[7]
            )
          : "",

      vendor:
        row[8]
          ? String(
              row[8]
            )
          : "OTHER",

      signatureText:
        row[9]
          ? String(
              row[9]
            )
          : "",

      approvalStep:
        Number(
          row[10] ||
          0
        ),

      statusText:
        row[11]
          ? String(
              row[11]
            )
          : "",

      statusStage:
        row[12]
          ? String(
              row[12]
            )
          : "PENDING"

    });

  }


  return result;

}



/*
 * ============================================================
 * STABLE ORDER KEY
 * ============================================================
 *
 * We avoid using only the row number because a row could move.
 *
 * The order identity uses:
 *
 * - original submission timestamp
 * - email
 * - student ID
 * - item
 * - event
 * - vendor URL
 *
 * If you ever add a permanent ORDER ID column, that would be
 * even better and we could use that instead.
 * ============================================================
 */

function buildTrackingOrderKey_(
  row,
  rowNumber
) {

  var formTimestamp =

    row[0] instanceof Date

      ? row[0]
          .toISOString()

      : String(
          row[0] ||
          ""
        ).trim();


  var stableParts =
    [

      formTimestamp,

      normalizeEmail(
        row[9]
      ),

      normalizeStudentId(
        row[11]
      ),

      row[3]
        ? row[3]
            .toString()
            .trim()
            .toLowerCase()
        : "",

      row[10]
        ? row[10]
            .toString()
            .trim()
            .toLowerCase()
        : "",

      row[4]
        ? row[4]
            .toString()
            .trim()
            .toLowerCase()
        : ""

    ];


  var meaningful =
    stableParts.join(
      "|"
    );


  /*
   * Extremely defensive fallback.
   */

  if (
    !meaningful.replace(
      /\|/g,
      ""
    )
  ) {

    meaningful =
      "row:" +
      rowNumber;

  }


  return sha256Hex_(
    meaningful
  );

}



/*
 * ============================================================
 * SHA-256 HELPER
 * ============================================================
 */

function sha256Hex_(
  value
) {

  var digest =
    Utilities.computeDigest(

      Utilities.DigestAlgorithm.SHA_256,

      String(
        value
      )

    );


  return digest

    .map(
      function(byte) {

        var n =
          byte < 0
            ? byte + 256
            : byte;


        var hex =
          n.toString(
            16
          );


        return hex.length ===
          1

          ? "0" + hex

          : hex;

      }
    )

    .join(
      ""
    );

}



/*
 * ============================================================
 * SIGNATURE → STEP
 * ============================================================
 */

function parseApprovalStep_(signatures) {

  var text =
    signatures
      ? signatures
          .toString()
          .toLowerCase()
          .trim()
      : "";


  // ========================================================
  // COMPLETION WORDING
  // ========================================================

  if (
    text.includes("signatures complete") ||
    text.includes("signature complete") ||
    text.includes("fully signed") ||
    text.includes("ready to order") ||
    text.includes("all signatures") ||
    text.includes("approval complete")
  ) {

    return 7;

  }


  // ========================================================
  // "STEP 3", "STEP 3/7", "STEP3/7"
  // ========================================================

  var match =
    text.match(
      /\bstep\s*([0-7])(?:\s*\/\s*7)?\b/i
    );


  if (match) {

    return Math.max(
      0,
      Math.min(
        7,
        Number(match[1])
      )
    );

  }


  // ========================================================
  // "3/7"
  // ========================================================

  match =
    text.match(
      /\b([0-7])\s*\/\s*7\b/
    );


  if (match) {

    return Math.max(
      0,
      Math.min(
        7,
        Number(match[1])
      )
    );

  }


  // ========================================================
  // "3 OF 7"
  // ========================================================

  match =
    text.match(
      /\b([0-7])\s+of\s+7\b/i
    );


  if (match) {

    return Math.max(
      0,
      Math.min(
        7,
        Number(match[1])
      )
    );

  }


  return 0;

}



/*
 * ============================================================
 * COLUMN S → ACTUAL STATUS STAGE
 * ============================================================
 *
 * Column S is authoritative.
 * ============================================================
 */

function normalizeStatusStage_(
  status
) {

  var text =
    status

      ? status
          .toString()
          .toLowerCase()
          .trim()

      : "";


  if (
    text.includes(
      "delivered"
    )
  ) {

    return "DELIVERED";

  }


  if (

    text.includes(
      "shipped"
    ) ||

    text.includes(
      "in transit"
    )

  ) {

    return "SHIPPED";

  }


  if (

    text.includes(
      "ordered"
    ) ||

    text.includes(
      "purchased"
    )

  ) {

    return "ORDERED";

  }


  if (
    text.includes(
      "cancel"
    )
  ) {

    return "CANCELLED";

  }


  return "PENDING";

}



/*
 * ============================================================
 * VENDOR DETECTION
 * ============================================================
 *
 * Amazon links receive the Amazon shipping model.
 *
 * Supports normal Amazon URLs and common short links.
 * ============================================================
 */

function detectVendorType_(
  link
) {

  var text =
    link

      ? link
          .toString()
          .toLowerCase()
          .trim()

      : "";


  if (!text) {

    return "OTHER";

  }


  var host =
    text

      .replace(
        /^https?:\/\//i,
        ""
      )

      .split(
        "/"
      )[0]

      .split(
        "?"
      )[0]

      .split(
        "#"
      )[0]

      .replace(
        /^www\./i,
        ""
      );


  if (

    host ===
      "amzn.to" ||

    host ===
      "a.co" ||

    host.indexOf(
      "amazon."
    ) ===
      0 ||

    host.indexOf(
      ".amazon."
    ) !==
      -1

  ) {

    return "AMAZON";

  }


  return "OTHER";

}



/*
 * ============================================================
 * BUILD TIMING MODEL
 * ============================================================
 *
 * This learns four things:
 *
 * 1. Hours per approval/signature step
 * 2. Signatures complete → ordered
 * 3. Ordered → delivered
 * 4. Shipped → delivered
 *
 * Approval timing uses BOTH:
 *
 * - robust median transition rates
 * - linear regression
 *
 * Outliers are filtered so one weird six-week approval does not
 * destroy the whole estimate.
 * ============================================================
 */

function buildDeliveryTimingModel_() {

  var history =
    readTrackingHistory_();


  var byOrder =
    {};


  var lastStateByOrder =
    {};


  history.forEach(
    function(entry) {

      if (
        !byOrder[
          entry.orderKey
        ]
      ) {

        byOrder[
          entry.orderKey
        ] =
          [];

      }


      byOrder[
        entry.orderKey
      ].push(
        entry
      );


      var old =
        lastStateByOrder[
          entry.orderKey
        ];


      if (

        !old ||

        entry.observedAt.getTime() >
        old.observedAt.getTime()

      ) {

        lastStateByOrder[
          entry.orderKey
        ] =
          entry;

      }

    }
  );


  /*
   * Individual learned samples.
   */

  var approvalRates =
    [];


  var regressionPoints =
    [];


  var signaturesToOrder =
    [];


  var orderedToDelivered =
    {

      AMAZON:
        [],

      OTHER:
        []

    };


  var shippedToDelivered =
    {

      AMAZON:
        [],

      OTHER:
        []

    };


  /*
   * Deduplication maps.
   *
   * If twenty items from one batch all advance from Step 2 to
   * Step 3 at the exact same scan time, that should count as ONE
   * approval-process observation, not twenty.
   */

  var transitionDedup =
    {};


  var regressionDedup =
    {};


  var postDedup =
    {};


  Object.keys(
    byOrder
  )

  .forEach(
    function(orderKey) {

      var events =
        byOrder[
          orderKey
        ]

        .slice()

        .sort(
          function(
            a,
            b
          ) {

            return (

              a.observedAt.getTime() -
              b.observedAt.getTime()

            );

          }
        );


      /*
       * ======================================================
       * APPROVAL TRANSITION SPEED
       * ======================================================
       */

      var approvalEvents =
        events.filter(
          function(e) {

            return (

              e.approvalStep >= 0 &&
              e.approvalStep <= 7

            );

          }
        );


      for (
        var i = 1;
        i < approvalEvents.length;
        i++
      ) {

        var previous =
          approvalEvents[
            i - 1
          ];


        var current =
          approvalEvents[
            i
          ];


        var deltaStep =

          current.approvalStep -
          previous.approvalStep;


        var deltaHours =
          hoursBetween_(

            previous.observedAt,

            current.observedAt

          );


        if (

          deltaStep > 0 &&
          deltaHours > 0

        ) {

          var rate =

            deltaHours /
            deltaStep;


          var dedupKey =

            previous.approvalStep +

            "->" +

            current.approvalStep +

            "|" +

            current.observedAt
              .getTime();


          if (
            !transitionDedup[
              dedupKey
            ]
          ) {

            transitionDedup[
              dedupKey
            ] =
              true;


            approvalRates.push(
              rate
            );

          }

        }

      }


      /*
       * ======================================================
       * STEP-vs-TIME REGRESSION DATA
       * ======================================================
       *
       * Each order starts at its first observed approval state.
       *
       * x = number of steps progressed
       * y = elapsed hours
       *
       * We fit:
       *
       * elapsed time ≈ slope × steps
       *
       * So slope = hours per approval step.
       * ======================================================
       */

      if (
        approvalEvents.length >=
        2
      ) {

        var first =
          approvalEvents[
            0
          ];


        for (
          var j = 1;
          j < approvalEvents.length;
          j++
        ) {

          var point =
            approvalEvents[
              j
            ];


          var x =

            point.approvalStep -
            first.approvalStep;


          var y =
            hoursBetween_(

              first.observedAt,

              point.observedAt

            );


          if (
            x > 0 &&
            y > 0
          ) {

            /*
             * Identical synchronized batches collapse into one
             * regression observation.
             */

            var pointKey =

              x +

              "|" +

              (
                Math.round(
                  y * 10
                ) /
                10
              );


            if (
              !regressionDedup[
                pointKey
              ]
            ) {

              regressionDedup[
                pointKey
              ] =
                true;


              regressionPoints.push({

                x:
                  x,

                y:
                  y

              });

            }

          }

        }

      }


      /*
       * ======================================================
       * POST-APPROVAL ACTUAL TIMING
       * ======================================================
       */

      var signatureComplete =
        firstEventMatching_(

          events,

          function(e) {

            return (
              e.approvalStep >=
              7
            );

          }

        );


      var ordered =
        firstEventMatching_(

          events,

          function(e) {

            return (
              e.statusStage ===
              "ORDERED"
            );

          }

        );


      var shipped =
        firstEventMatching_(

          events,

          function(e) {

            return (
              e.statusStage ===
              "SHIPPED"
            );

          }

        );


      var delivered =
        firstEventMatching_(

          events,

          function(e) {

            return (
              e.statusStage ===
              "DELIVERED"
            );

          }

        );


      var vendor =

        events.length

          ? events[
              events.length -
              1
            ].vendor

          : "OTHER";


      if (
        vendor !==
        "AMAZON"
      ) {

        vendor =
          "OTHER";

      }


      /*
       * SIGNATURES COMPLETE → ORDERED
       */

      if (

        signatureComplete &&
        ordered &&

        ordered.observedAt.getTime() >
        signatureComplete.observedAt.getTime()

      ) {

        var h1 =
          hoursBetween_(

            signatureComplete.observedAt,

            ordered.observedAt

          );


        var k1 =

          "sig-order|" +

          ordered.observedAt
            .getTime();


        if (
          !postDedup[
            k1
          ]
        ) {

          postDedup[
            k1
          ] =
            true;


          signaturesToOrder.push(
            h1
          );

        }

      }


      /*
       * ORDERED → DELIVERED
       */

      if (

        ordered &&
        delivered &&

        delivered.observedAt.getTime() >
        ordered.observedAt.getTime()

      ) {

        var h2 =
          hoursBetween_(

            ordered.observedAt,

            delivered.observedAt

          );


        var k2 =

          vendor +

          "|order-delivered|" +

          delivered.observedAt
            .getTime();


        if (
          !postDedup[
            k2
          ]
        ) {

          postDedup[
            k2
          ] =
            true;


          orderedToDelivered[
            vendor
          ].push(
            h2
          );

        }

      }


      /*
       * SHIPPED → DELIVERED
       */

      if (

        shipped &&
        delivered &&

        delivered.observedAt.getTime() >
        shipped.observedAt.getTime()

      ) {

        var h3 =
          hoursBetween_(

            shipped.observedAt,

            delivered.observedAt

          );


        var k3 =

          vendor +

          "|shipped-delivered|" +

          delivered.observedAt
            .getTime();


        if (
          !postDedup[
            k3
          ]
        ) {

          postDedup[
            k3
          ] =
            true;


          shippedToDelivered[
            vendor
          ].push(
            h3
          );

        }

      }

    }
  );


  /*
   * ==========================================================
   * ROBUST APPROVAL MODEL
   * ==========================================================
   */


  var filteredRates =
    robustFilterHours_(

      approvalRates,

      1,

      336

    );


  var medianRate =
    median_(
      filteredRates
    );


  var regression =
    regressionSlopeThroughOrigin_(
      regressionPoints
    );


  /*
   * Start with 3 days per step until we learn enough.
   */

  var approvalHoursPerStep =
    APPROVAL_FALLBACK_HOURS_PER_STEP;


  /*
   * Median is intentionally weighted more than regression.
   *
   * Approval systems sometimes have large pauses and outliers,
   * so a pure least-squares model can be distorted badly.
   */

  if (

    filteredRates.length >=
      2 &&

    regression.slope >
      0

  ) {

    var regressionWeight =

      regressionPoints.length >=
        5 &&

      regression.r2 >=
        0.55

        ? 0.35

        : 0.15;


    approvalHoursPerStep =

      (
        1 -
        regressionWeight
      ) *
      medianRate +

      regressionWeight *
      regression.slope;

  }

  else if (
    filteredRates.length >=
    1
  ) {

    approvalHoursPerStep =
      medianRate;

  }

  else if (
    regression.slope >
    0
  ) {

    approvalHoursPerStep =
      regression.slope;

  }


  /*
   * Prevent pathological data from giving ridiculous models.
   *
   * Minimum: 6 hr / step
   * Maximum: 7 days / step
   */

  approvalHoursPerStep =
    clamp_(

      approvalHoursPerStep,

      6,

      168

    );


  /*
   * ==========================================================
   * MODEL CONFIDENCE
   * ==========================================================
   */

  var confidence =
    "low";


  if (

    filteredRates.length >=
      10 &&

    regressionPoints.length >=
      8 &&

    regression.r2 >=
      0.55

  ) {

    confidence =
      "high";

  }

  else if (

    filteredRates.length >=
      4 ||

    regressionPoints.length >=
      5

  ) {

    confidence =
      "medium";

  }


  /*
   * ==========================================================
   * FINAL MODEL
   * ==========================================================
   */

  return {

    approvalHoursPerStep:
      approvalHoursPerStep,


    approvalSamples:
      filteredRates.length,


    regressionSamples:
      regressionPoints.length,


    regressionR2:
      regression.r2,


    confidence:
      confidence,


    /*
     * If we have real history, use its median.
     *
     * Otherwise:
     * signatures → order = 3 days.
     */

    signaturesToOrderHours:

      robustMedianOrFallback_(

        signaturesToOrder,

        SIGNATURES_TO_ORDER_FALLBACK_HOURS,

        1,

        336

      ),


    /*
     * Ordered → Delivered
     */

    orderedToDeliveredHours: {

      AMAZON:

        robustMedianOrFallback_(

          orderedToDelivered.AMAZON,

          AMAZON_ORDER_TO_DELIVERED_FALLBACK_HOURS,

          6,

          720

        ),


      OTHER:

        robustMedianOrFallback_(

          orderedToDelivered.OTHER,

          OTHER_ORDER_TO_DELIVERED_FALLBACK_HOURS,

          6,

          1008

        )

    },


    /*
     * Shipped → Delivered
     */

    shippedToDeliveredHours: {

      AMAZON:

        robustMedianOrFallback_(

          shippedToDelivered.AMAZON,

          AMAZON_SHIPPED_TO_DELIVERED_FALLBACK_HOURS,

          3,

          336

        ),


      OTHER:

        robustMedianOrFallback_(

          shippedToDelivered.OTHER,

          OTHER_SHIPPED_TO_DELIVERED_FALLBACK_HOURS,

          3,

          504

        )

    },

    sampleCounts: {

      approval:
        filteredRates.length,

      regression:
        regressionPoints.length,

      signaturesToOrder:
        robustFilterHours_(
          signaturesToOrder,
          1,
          336
        ).length,

      orderedToDelivered: {

        AMAZON:
          robustFilterHours_(
            orderedToDelivered.AMAZON,
            6,
            720
          ).length,

        OTHER:
          robustFilterHours_(
            orderedToDelivered.OTHER,
            6,
            1008
          ).length

      },

      shippedToDelivered: {

        AMAZON:
          robustFilterHours_(
            shippedToDelivered.AMAZON,
            3,
            336
          ).length,

        OTHER:
          robustFilterHours_(
            shippedToDelivered.OTHER,
            3,
            504
          ).length

      }

    },
    /*
     * Used to determine when the CURRENT stage began.
     */

    lastStateByOrder:
      lastStateByOrder

  };

}



/*
 * ============================================================
 * MAIN ESTIMATOR
 * ============================================================
 *
 * Keep this function name because the rest of your existing
 * project already expects calculateVelocityDelivery().
 * ============================================================
 */

function calculateVelocityDelivery(

  signatures,

  orderStatus,

  link,

  orderKey,

  timingModel

) {

  /*
   * Passing timingModel from fetchRowsForEmail() is much faster
   * than rebuilding the regression separately for every card.
   */

  timingModel =
    timingModel ||
    buildDeliveryTimingModel_();


  var now =
    new Date();


  var currentStep =
    parseApprovalStep_(
      signatures
    );


  var statusStage =
    normalizeStatusStage_(
      orderStatus
    );


  var vendor =
    detectVendorType_(
      link
    );


  /*
   * Find when this order entered its current stage.
   */

  var latest =
    timingModel
      .lastStateByOrder[
        orderKey
      ];


  var stageStartedAt =

    latest &&
    latest.observedAt instanceof Date

      ? latest.observedAt

      : now;



  /*
   * ==========================================================
   * DELIVERED
   *
   * Column S wins over absolutely everything.
   * ==========================================================
   */

  if (
    statusStage ===
    "DELIVERED"
  ) {

    return {

      text:
        "Delivered",

      styleClass:
        "arrival-delivered",

      isStuck:
        false,

      step:
        7,

      isDelivered:
        true,

      stage:
        "DELIVERED",

      vendor:
        vendor,

      estimatedDeliveryAt:
        null,

      nextSignatureAt:
        null,

      confidence:
        timingModel.confidence,

      confidence:
        "actual",

      confidenceLabel:
        "Actual",

      confidenceDetail:
        "Delivery confirmed by the current order status."

    };

  }



  /*
   * ==========================================================
   * CANCELLED
   * ==========================================================
   */

  if (
    statusStage ===
    "CANCELLED"
  ) {

    return {

      text:
        "Cancelled",

      styleClass:
        "arrival-delayed",

      isStuck:
        false,

      step:
        currentStep,

      isDelivered:
        false,

      stage:
        "CANCELLED",

      vendor:
        vendor,

      estimatedDeliveryAt:
        null,

      nextSignatureAt:
        null,

      confidence:
        timingModel.confidence

    };

  }



  var deliveryHours;

  var estimatedDeliveryAt;

  var nextSignatureAt =
    null;

  var text;

  var stage;



  /*
   * ==========================================================
   * ALREADY SHIPPED
   * ==========================================================
   */

  if (
    statusStage ===
    "SHIPPED"
  ) {

    deliveryHours =

      timingModel
        .shippedToDeliveredHours[
          vendor
        ];


    estimatedDeliveryAt =
      addHours_(

        stageStartedAt,

        deliveryHours

      );


    stage =
      "SHIPPED";


    text =

      "Shipped • est. " +

      remainingTimeText_(

        estimatedDeliveryAt,

        now

      );

  }


  /*
   * ==========================================================
   * ALREADY ORDERED
   * ==========================================================
   */

  else if (
    statusStage ===
    "ORDERED"
  ) {

    deliveryHours =

      timingModel
        .orderedToDeliveredHours[
          vendor
        ];


    estimatedDeliveryAt =
      addHours_(

        stageStartedAt,

        deliveryHours

      );


    stage =
      "ORDERED";


    text =

      "Ordered • est. " +

      remainingTimeText_(

        estimatedDeliveryAt,

        now

      );

  }


  /*
   * ==========================================================
   * SIGNATURES COMPLETE
   *
   * Approval is over.
   *
   * Prediction:
   *
   * current time
   * + learned signatures→order
   * + learned vendor delivery time
   * ==========================================================
   */

  else if (
    currentStep >=
    7
  ) {

    var orderDelay =

      timingModel
        .signaturesToOrderHours;


    deliveryHours =

      timingModel
        .orderedToDeliveredHours[
          vendor
        ];


    estimatedDeliveryAt =
      addHours_(

        stageStartedAt,

        orderDelay +
        deliveryHours

      );


    stage =
      "SIGNATURES_COMPLETE";


    text =

      "Signatures complete • est. " +

      remainingTimeText_(

        estimatedDeliveryAt,

        now

      );

  }


  /*
   * ==========================================================
   * STILL IN APPROVAL PROCESS
   * ==========================================================
   */

  else {

    var hoursPerStep =

      timingModel
        .approvalHoursPerStep;


    var remainingApprovalSteps =

      Math.max(

        0,

        7 -
        currentStep

      );


    var approvalHoursRemaining =

      remainingApprovalSteps *
      hoursPerStep;


    /*
     * After approvals:
     *
     * signatures complete
     *      ↓
     * wait to be ordered
     *      ↓
     * shipping
     */

    var afterApprovalHours =

      timingModel
        .signaturesToOrderHours +

      timingModel
        .orderedToDeliveredHours[
          vendor
        ];


    nextSignatureAt =
      addHours_(

        stageStartedAt,

        hoursPerStep

      );


    estimatedDeliveryAt =
      addHours_(

        stageStartedAt,

        approvalHoursRemaining +
        afterApprovalHours

      );


    stage =
      "APPROVAL";


    text =

      "~" +

      remainingTimeText_(

        estimatedDeliveryAt,

        now

      ) +

      " remaining";


    /*
     * Once we actually have a signature step, additionally show
     * when the next approval is expected.
     */

    if (
      currentStep >
      0
    ) {

      text +=

        " • next signature ~" +

        remainingTimeText_(

          nextSignatureAt,

          now

        );

    }

  }



  /*
   * ==========================================================
   * STUCK / DELAY DETECTION
   * ==========================================================
   *
   * Instead of a fixed "stuck after N days", this threshold
   * scales with the learned timing model.
   * ==========================================================
   */

  var stageAgeHours =
    hoursBetween_(

      stageStartedAt,

      now

    );


  var stuckThresholdHours;


  if (
    stage ===
    "APPROVAL"
  ) {

    stuckThresholdHours =

      Math.max(

        72,

        timingModel
          .approvalHoursPerStep *
        2.25

      );

  }

  else if (
    stage ===
    "SIGNATURES_COMPLETE"
  ) {

    stuckThresholdHours =

      Math.max(

        96,

        timingModel
          .signaturesToOrderHours *
        2.25

      );

  }

  else if (
    stage ===
    "ORDERED"
  ) {

    stuckThresholdHours =

      Math.max(

        168,

        timingModel
          .orderedToDeliveredHours[
            vendor
          ] *
        1.8

      );

  }

  else if (
    stage ===
    "SHIPPED"
  ) {

    stuckThresholdHours =

      Math.max(

        96,

        timingModel
          .shippedToDeliveredHours[
            vendor
          ] *
        2

      );

  }

  else {

    stuckThresholdHours =
      999999;

  }


  var isStuck =

    stageAgeHours >
    stuckThresholdHours;



  /*
   * ==========================================================
   * DISPLAY STYLE
   * ==========================================================
   */

  var daysRemaining =

    Math.max(

      0,

      hoursBetween_(

        now,

        estimatedDeliveryAt

      ) /
      24

    );


  var styleClass =
    "arrival-medium";


  if (
    isStuck
  ) {

    styleClass =
      "arrival-delayed";

  }

  else if (
    daysRemaining <=
    5
  ) {

    styleClass =
      "arrival-soon";

  }


  var confidenceInfo =
    getEstimateConfidence_(
      stage,
      vendor,
      timingModel
    );
  /*
   * ==========================================================
   * RESULT
   * ==========================================================
   */

  return {

    /*
     * Existing frontend already reads this.
     */

    text:
      text,


    styleClass:
      styleClass,


    isStuck:
      isStuck,


    step:
      currentStep,


    /*
     * Extra information that can be useful later.
     */

    isDelivered:
      false,


    stage:
      stage,


    vendor:
      vendor,


    estimatedDeliveryAt:

      estimatedDeliveryAt

        ? estimatedDeliveryAt
            .toISOString()

        : null,


    nextSignatureAt:

      nextSignatureAt

        ? nextSignatureAt
            .toISOString()

        : null,


    confidence:
      confidenceInfo.level,

    confidenceLabel:
      confidenceInfo.label,

    confidenceDetail:
      confidenceInfo.detail,


    modelApprovalHoursPerStep:
      timingModel
        .approvalHoursPerStep,


    regressionR2:
      timingModel
        .regressionR2,


    approvalSamples:
      timingModel
        .approvalSamples

  };

}



/*
 * ============================================================
 * FIRST MATCHING EVENT
 * ============================================================
 */

function firstEventMatching_(
  events,
  predicate
) {

  for (
    var i = 0;
    i < events.length;
    i++
  ) {

    if (
      predicate(
        events[i]
      )
    ) {

      return events[i];

    }

  }


  return null;

}



/*
 * ============================================================
 * TIME HELPERS
 * ============================================================
 */

function hoursBetween_(
  a,
  b
) {

  return (

    b.getTime() -
    a.getTime()

  ) /
  3600000;

}


function addHours_(
  date,
  hours
) {

  return new Date(

    date.getTime() +

    hours *
    3600000

  );

}



/*
 * ============================================================
 * HUMAN-READABLE TIME
 * ============================================================
 */

function remainingTimeText_(
  future,
  now
) {

  var hours =

    Math.max(

      0,

      hoursBetween_(
        now,
        future
      )

    );


  if (
    hours <
    24
  ) {

    var roundedHours =
      Math.max(

        1,

        Math.round(
          hours
        )

      );


    return (

      roundedHours +

      " hr" +

      (
        roundedHours ===
        1

          ? ""

          : "s"
      )

    );

  }


  var days =
    hours /
    24;


  if (
    days <
    10
  ) {

    return (

      Math.round(
        days *
        10
      ) /
      10

    ) +
    " days";

  }


  return (

    Math.round(
      days
    ) +

    " days"

  );

}



/*
 * ============================================================
 * REGRESSION
 * ============================================================
 *
 * Fits:
 *
 * elapsedHours = slope × stepsProgressed
 *
 * through the origin.
 *
 * slope therefore means:
 *
 * average learned hours per signature step.
 * ============================================================
 */

function regressionSlopeThroughOrigin_(
  points
) {

  if (
    !points ||
    points.length ===
    0
  ) {

    return {

      slope:
        0,

      r2:
        0

    };

  }


  var sumXY =
    0;


  var sumXX =
    0;


  points.forEach(
    function(p) {

      sumXY +=
        p.x *
        p.y;


      sumXX +=
        p.x *
        p.x;

    }
  );


  if (
    sumXX ===
    0
  ) {

    return {

      slope:
        0,

      r2:
        0

    };

  }


  var slope =

    sumXY /
    sumXX;


  var meanY =

    points.reduce(

      function(
        sum,
        p
      ) {

        return (
          sum +
          p.y
        );

      },

      0

    ) /
    points.length;


  var ssRes =
    0;


  var ssTot =
    0;


  points.forEach(
    function(p) {

      var predicted =

        slope *
        p.x;


      ssRes +=

        Math.pow(

          p.y -
          predicted,

          2

        );


      ssTot +=

        Math.pow(

          p.y -
          meanY,

          2

        );

    }
  );


  var r2 =

    ssTot >
    0

      ? 1 -
        ssRes /
        ssTot

      : 0;


  return {

    slope:
      slope,

    r2:
      r2

  };

}



/*
 * ============================================================
 * ROBUST STATISTICS
 * ============================================================
 */

function robustMedianOrFallback_(

  values,

  fallback,

  minHours,

  maxHours

) {

  var filtered =
    robustFilterHours_(

      values,

      minHours,

      maxHours

    );


  return filtered.length

    ? median_(
        filtered
      )

    : fallback;

}



/*
 * Removes obviously impossible values and then applies the
 * standard 1.5 × IQR outlier rule.
 */

function robustFilterHours_(

  values,

  minHours,

  maxHours

) {

  var clean =

    (
      values ||
      []
    )

    .filter(
      function(v) {

        return (

          typeof v ===
            "number" &&

          isFinite(
            v
          ) &&

          v >=
            minHours &&

          v <=
            maxHours

        );

      }
    )

    .sort(
      function(
        a,
        b
      ) {

        return (
          a -
          b
        );

      }
    );


  /*
   * Too few samples to meaningfully remove outliers.
   */

  if (
    clean.length <
    4
  ) {

    return clean;

  }


  var q1 =
    percentile_(
      clean,
      0.25
    );


  var q3 =
    percentile_(
      clean,
      0.75
    );


  var iqr =
    q3 -
    q1;


  var low =

    q1 -
    1.5 *
    iqr;


  var high =

    q3 +
    1.5 *
    iqr;


  return clean.filter(
    function(v) {

      return (

        v >= low &&
        v <= high

      );

    }
  );

}



/*
 * ============================================================
 * MEDIAN
 * ============================================================
 */

function median_(
  values
) {

  if (
    !values ||
    values.length ===
    0
  ) {

    return 0;

  }


  var arr =
    values

      .slice()

      .sort(
        function(
          a,
          b
        ) {

          return (
            a -
            b
          );

        }
      );


  var middle =
    Math.floor(

      arr.length /
      2

    );


  if (
    arr.length %
    2
  ) {

    return arr[
      middle
    ];

  }


  return (

    arr[
      middle -
      1
    ] +

    arr[
      middle
    ]

  ) /
  2;

}



/*
 * ============================================================
 * PERCENTILE
 * ============================================================
 */

function percentile_(
  sortedValues,
  p
) {

  if (
    !sortedValues.length
  ) {

    return 0;

  }


  var index =

    (
      sortedValues.length -
      1
    ) *
    p;


  var lower =
    Math.floor(
      index
    );


  var upper =
    Math.ceil(
      index
    );


  if (
    lower ===
    upper
  ) {

    return sortedValues[
      lower
    ];

  }


  var weight =

    index -
    lower;


  return (

    sortedValues[
      lower
    ] *
    (
      1 -
      weight
    )

  ) +

  (

    sortedValues[
      upper
    ] *
    weight

  );

}



/*
 * ============================================================
 * CLAMP
 * ============================================================
 */

function clamp_(
  value,
  minValue,
  maxValue
) {

  return Math.max(

    minValue,

    Math.min(

      maxValue,

      value

    )

  );

}


/*
 * ============================================================
 * TEAM STATISTICS
 * ============================================================
 *
 * Uses:
 *
 * C = Name
 * D = Item
 * F = Price
 * H = Quantity
 * J = Email
 * K = Event
 * L = Student ID
 * S = Status
 */

function fetchTeamStatistics() {

  var ss =
    SpreadsheetApp.openById(
      SPREADSHEET_ID
    );


  var sheet =
    ss.getSheetByName(
      "Master"
    );


  if (!sheet) {

    throw new Error(
      'Sheet "Master" was not found.'
    );

  }


  var data =
    sheet
      .getDataRange()
      .getValues();


  var identityGroups =
    buildPersonIdentityGroups(
      data
    );


  var approvedExternalEmails =
    getApprovedExternalEmailMap_();


  var totalOrders = 0;
  var totalQuantity = 0;
  var totalCost = 0;


  /*
   * people[personKey]
   *
   * personKey is the unioned identity-group root, NOT an email.
   */
  var people = {};


  var events = {};
  var statusCounts = {};
  var highValueOrders = [];


  // ==========================================================
  // PROCESS EACH ROW
  // ==========================================================

  for (
    var i = 1;
    i < data.length;
    i++
  ) {

    var row =
      data[i];


    // ========================================================
    // COLUMN J = EMAIL
    // ========================================================

    var email =
      normalizeEmail(
        row[9]
      );


    if (!email) {

      continue;

    }


    if (
      !isOrderEmailAllowed_(
        email,
        approvedExternalEmails
      )
    ) {

      continue;

    }


    // ========================================================
    // COLUMN C = NAME
    // ========================================================

    var personName =
      row[2]
        ? row[2]
            .toString()
            .trim()
        : "Unknown";


    var normalizedName =
      normalizePersonName(
        personName
      );


    // ========================================================
    // COLUMN L = STUDENT ID
    // ========================================================

    var studentIdSuffix =
      normalizeStudentId(
        row[11]
      );


    /*
     * All rows in the same identity cluster receive the same key.
     * Same email always joins clusters; otherwise a matching
     * Student-ID suffix joins them.  Name alone never does.
     */
    var personKey =
      "person_" +
      identityGroups.find(i);


    // ========================================================
    // COLUMN D = ITEM
    // ========================================================

    var itemName =
      row[3]
        ? row[3]
            .toString()
            .trim()
        : "Unnamed Item";


    // ========================================================
    // COLUMN F = PRICE
    // ========================================================

    var priceRaw =
      row[5]
        ? row[5]
            .toString()
            .trim()
        : "";


    var price =
      parsePrice(
        priceRaw
      );


    // ========================================================
    // COLUMN H = QUANTITY
    // ========================================================

    var quantity =
      row[7]
        ? row[7]
            .toString()
            .trim()
        : "1";


    var numericQuantity =
      parseFloat(
        quantity
      );

    if (
      isNaN(numericQuantity) ||
      numericQuantity <= 0
    ) {
      numericQuantity = 1;
    }

    price =
      price *
      numericQuantity;


    // ========================================================
    // COLUMN K = EVENT
    // ========================================================

    var eventName =
      row[10]
        ? row[10]
            .toString()
            .trim()
        : "Unassigned Event";


    // ========================================================
    // COLUMN S = ACTUAL STATUS
    // ========================================================

    var status =
      row[18]
        ? row[18]
            .toString()
            .trim()
        : "Pending";


    // ========================================================
    // OVERALL TOTALS
    // ========================================================

    totalOrders++;
    totalCost += price;


    if (
      !isNaN(
        numericQuantity
      )
    ) {

      totalQuantity +=
        numericQuantity;

    }


    // ========================================================
    // STATUS COUNTS
    // ========================================================

    if (
      !statusCounts[status]
    ) {

      statusCounts[status] = 0;

    }


    statusCounts[status]++;


    // ========================================================
    // HIGH-VALUE ORDER
    // ========================================================

    if (
      price >
      HIGH_VALUE_THRESHOLD
    ) {

      highValueOrders.push({

        rowNum:
          i + 1,

        email:
          email,

        name:
          personName,

        studentIdSuffix:
          studentIdSuffix,

        itemName:
          itemName,

        price:
          price,

        quantity:
          quantity,

        eventName:
          eventName,

        status:
          status

      });

    }


    // ========================================================
    // PERSON
    // ========================================================

    if (
      !people[personKey]
    ) {

      people[personKey] = {

        personKey:
          personKey,

        email:
          email,

        emails:
          {},

        studentIds:
          {},

        names:
          {},

        name:
          personName,

        totalCost:
          0,

        orderCount:
          0,

        items:
          []

      };

    }


    people[personKey]
      .emails[email] = true;


    if (studentIdSuffix) {

      people[personKey]
        .studentIds[studentIdSuffix] = true;

    }


    if (normalizedName) {

      people[personKey]
        .names[normalizedName] = true;

    }


    if (
      personName &&
      personName !== "Unknown"
    ) {

      people[personKey].name =
        personName;

    }


    people[personKey].totalCost +=
      price;


    people[personKey].orderCount++;


    people[personKey].items.push({

      itemName:
        itemName,

      price:
        price,

      quantity:
        quantity,

      eventName:
        eventName,

      status:
        status

    });


    // ========================================================
    // EVENT
    // ========================================================

    if (
      !events[eventName]
    ) {

      events[eventName] = {

        eventName:
          eventName,

        totalCost:
          0,

        orderCount:
          0,

        people:
          {}

      };

    }


    events[eventName].totalCost +=
      price;


    events[eventName].orderCount++;


    if (
      !events[eventName]
        .people[personKey]
    ) {

      events[eventName]
        .people[personKey] = {

          personKey:
            personKey,

          email:
            email,

          name:
            personName,

          totalCost:
            0,

          items:
            []

        };

    }


    if (
      personName &&
      personName !== "Unknown"
    ) {

      events[eventName]
        .people[personKey]
        .name =
          personName;

    }


    events[eventName]
      .people[personKey]
      .totalCost +=
        price;


    events[eventName]
      .people[personKey]
      .items.push({

        itemName:
          itemName,

        price:
          price,

        quantity:
          quantity

      });

  }


  // ==========================================================
  // SEND HIGH-VALUE EMAIL ALERTS
  // ==========================================================

  sendNewHighValueOrderAlerts(
    highValueOrders
  );


  // ==========================================================
  // PEOPLE ARRAY
  // ==========================================================

  var peopleList =
    Object.keys(
      people
    )
    .map(
      function(personKey) {

        return {

          personKey:
            people[personKey].personKey,

          /*
           * Keep one representative email for backwards
           * compatibility.  Identity is no longer keyed by it.
           */
          email:
            people[personKey].email,

          name:
            people[personKey].name,

          totalCost:
            people[personKey].totalCost,

          orderCount:
            people[personKey].orderCount,

          items:
            people[personKey].items

        };

      }
    )
    .sort(
      function(a, b) {

        return (
          b.totalCost -
          a.totalCost
        );

      }
    );


  // ==========================================================
  // EVENT ARRAY
  // ==========================================================

  var eventList =
    Object.keys(
      events
    )
    .map(
      function(eventName) {

        var event =
          events[eventName];


        var eventPeople =
          Object.keys(
            event.people
          )
          .map(
            function(personKey) {

              return {

                personKey:
                  event
                    .people[personKey]
                    .personKey,

                email:
                  event
                    .people[personKey]
                    .email,

                name:
                  event
                    .people[personKey]
                    .name,

                totalCost:
                  event
                    .people[personKey]
                    .totalCost,

                items:
                  event
                    .people[personKey]
                    .items

              };

            }
          )
          .sort(
            function(a, b) {

              return (
                b.totalCost -
                a.totalCost
              );

            }
          );


        return {

          eventName:
            event.eventName,

          totalCost:
            event.totalCost,

          orderCount:
            event.orderCount,

          people:
            eventPeople

        };

      }
    )
    .sort(
      function(a, b) {

        return (
          b.totalCost -
          a.totalCost
        );

      }
    );


  return {

    totalOrders:
      totalOrders,

    totalStudents:
      peopleList.length,

    totalQuantity:
      totalQuantity,

    totalCost:
      totalCost,

    statusCounts:
      statusCounts,

    events:
      eventList,

    people:
      peopleList,

    highValueOrders:
      highValueOrders

  };

}


/*
 * ============================================================
 * HIGH-VALUE EMAIL ALERTS
 * ============================================================
 *
 * Sends an email for newly detected orders over $50.
 *
 * PropertiesService remembers previously alerted orders
 * so opening Team Statistics repeatedly does not spam
 * the recipient.
 */

function sendNewHighValueOrderAlerts(
  highValueOrders
) {

  if (
    !highValueOrders ||
    highValueOrders.length === 0
  ) {

    return;

  }


  var properties =
    PropertiesService
      .getScriptProperties();


  var alertKeys =
    JSON.parse(
      properties.getProperty(
        "HIGH_VALUE_ALERT_KEYS"
      ) || "{}"
    );


  var newOrders =
    [];


  highValueOrders.forEach(
    function(order) {

      /*
       * Create a stable internal identifier for
       * this particular high-value order.
       */

      var rawKey =
        [
          order.rowNum,
          order.email,
          order.itemName,
          order.price,
          order.eventName
        ].join("|");


      var digest =
        Utilities.computeDigest(
          Utilities.DigestAlgorithm.SHA_256,
          rawKey
        );


      var key =
        digest
          .map(
            function(byte) {

              var hex =
                (
                  byte < 0
                    ? byte + 256
                    : byte
                )
                .toString(16);


              return (
                hex.length === 1
                  ? "0" + hex
                  : hex
              );

            }
          )
          .join("");


      if (
        !alertKeys[key]
      ) {

        newOrders.push({

          order:
            order,

          key:
            key

        });

      }

    }
  );


  if (
    newOrders.length === 0
  ) {

    return;

  }


  // ==========================================================
  // EMAIL SUBJECT
  // ==========================================================

  var subject =
    newOrders.length === 1
      ? "SciOly High-Value Order Alert"
      : "SciOly High-Value Order Alerts";


  // ==========================================================
  // EMAIL BODY
  // ==========================================================

  var body =
    "SciOly Tracking Portal\n" +
    "High-Value Order Alert\n\n" +
    "The following order(s) are over $" +
    HIGH_VALUE_THRESHOLD +
    ".\n\n";


  newOrders.forEach(
    function(entry, index) {

      var order =
        entry.order;


      body +=

        "----------------------------------------\n" +

        "Alert #" +
        (index + 1) +
        "\n\n" +

        "Student: " +
        order.name +
        "\n" +

        "Email: " +
        order.email +
        "\n" +

        "Item: " +
        order.itemName +
        "\n" +

        "Event: " +
        order.eventName +
        "\n" +

        "Price: $" +
        order.price.toFixed(2) +
        "\n" +

        "Quantity: " +
        order.quantity +
        "\n" +

        "Status: " +
        order.status +
        "\n\n";

    }
  );


  body +=
    "This message was automatically generated by " +
    "the SciOly Tracking Portal.";


  // ==========================================================
  // SEND
  // ==========================================================

  MailApp.sendEmail(
    HIGH_VALUE_ALERT_EMAIL,
    subject,
    body
  );


  // ==========================================================
  // REMEMBER ALERTED ORDERS
  // ==========================================================

  newOrders.forEach(
    function(entry) {

      alertKeys[
        entry.key
      ] =
        new Date().toISOString();

    }
  );


  properties.setProperty(
    "HIGH_VALUE_ALERT_KEYS",
    JSON.stringify(
      alertKeys
    )
  );

}


/*
 * ============================================================
 * RESET HIGH-VALUE ALERT HISTORY
 * ============================================================
 *
 * Only run this manually if you intentionally want to make
 * the system treat all current high-value orders as new.
 */

function resetHighValueAlertHistory() {

  PropertiesService
    .getScriptProperties()
    .deleteProperty(
      "HIGH_VALUE_ALERT_KEYS"
    );

}


/*
 * ============================================================
 * ONE-TIME AUTHORIZATION HELPER
 * ============================================================
 *
 * Run this manually once while signed into the Google account
 * that will deploy/execute the web app. It requests the same
 * Google-service permissions the portal needs without sending
 * a high-value alert email.
 */
function authorizeServices() {

  SpreadsheetApp
    .openById(
      SPREADSHEET_ID
    )
    .getName();


  MailApp
    .getRemainingDailyQuota();


  UrlFetchApp.fetch(
    "https://www.google.com/generate_204",
    {
      muteHttpExceptions: true
    }
  );


  Logger.log(
    "Authorization check completed."
  );

}

function checkHighValueOrders() {
  fetchTeamStatistics();
}

function getEstimateConfidence_(
  stage,
  vendor,
  timingModel
) {

  var counts =
    timingModel.sampleCounts || {};


  // ========================================================
  // APPROVAL STAGE
  // ========================================================

  if (
    stage === "APPROVAL"
  ) {

    var approvalSamples =
      Number(
        counts.approval || 0
      );


    if (
      approvalSamples >= 10
    ) {

      return {
        level: "high",
        label: "High",
        detail:
          "Based on " +
          approvalSamples +
          " historical approval transitions."
      };

    }


    if (
      approvalSamples >= 4
    ) {

      return {
        level: "medium",
        label: "Medium",
        detail:
          "Based on " +
          approvalSamples +
          " historical approval transitions."
      };

    }


    if (
      approvalSamples > 0
    ) {

      return {
        level: "low",
        label: "Low",
        detail:
          "Only " +
          approvalSamples +
          " historical approval transition" +
          (
            approvalSamples === 1
              ? ""
              : "s"
          ) +
          " recorded so far."
      };

    }


    return {
      level: "low",
      label: "Low",
      detail:
        "Using the default approval timing while the system learns."
    };

  }


  // ========================================================
  // SIGNATURES COMPLETE
  // ========================================================

  if (
    stage === "SIGNATURES_COMPLETE"
  ) {

    var signatureSamples =
      Number(
        counts.signaturesToOrder || 0
      );


    var deliverySamples =
      Number(
        (
          counts.orderedToDelivered || {}
        )[vendor] || 0
      );


    var usableSamples =
      Math.min(
        signatureSamples,
        deliverySamples
      );


    if (
      usableSamples >= 8
    ) {

      return {
        level: "high",
        label: "High",
        detail:
          "Based on historical ordering and delivery timing."
      };

    }


    if (
      usableSamples >= 3
    ) {

      return {
        level: "medium",
        label: "Medium",
        detail:
          "Based on a limited amount of ordering and delivery history."
      };

    }


    return {
      level: "low",
      label: "Low",
      detail:
        "Part of this estimate still uses default ordering or delivery timing."
    };

  }


  // ========================================================
  // ORDERED
  // ========================================================

  if (
    stage === "ORDERED"
  ) {

    var orderedSamples =
      Number(
        (
          counts.orderedToDelivered || {}
        )[vendor] || 0
      );


    if (
      orderedSamples >= 8
    ) {

      return {
        level: "high",
        label: "High",
        detail:
          "Based on " +
          orderedSamples +
          " previous " +
          vendor.toLowerCase() +
          " deliveries."
      };

    }


    if (
      orderedSamples >= 3
    ) {

      return {
        level: "medium",
        label: "Medium",
        detail:
          "Based on " +
          orderedSamples +
          " previous deliveries."
      };

    }


    if (
      orderedSamples > 0
    ) {

      return {
        level: "low",
        label: "Low",
        detail:
          "Only " +
          orderedSamples +
          " previous delivery" +
          (
            orderedSamples === 1
              ? ""
              : "ies"
          ) +
          " recorded."
      };

    }


    return {
      level: "low",
      label: "Low",
      detail:
        vendor === "AMAZON"
          ? "Using the default 5-day Amazon estimate."
          : "Using the default 7-day vendor estimate."
    };

  }


  // ========================================================
  // SHIPPED
  // ========================================================

  if (
    stage === "SHIPPED"
  ) {

    var shippedSamples =
      Number(
        (
          counts.shippedToDelivered || {}
        )[vendor] || 0
      );


    if (
      shippedSamples >= 8
    ) {

      return {
        level: "high",
        label: "High",
        detail:
          "Based on " +
          shippedSamples +
          " previous shipped-to-delivered times."
      };

    }


    if (
      shippedSamples >= 3
    ) {

      return {
        level: "medium",
        label: "Medium",
        detail:
          "Based on a limited amount of shipping history."
      };

    }


    return {
      level: "low",
      label: "Low",
      detail:
        "Using mostly default shipping timing while more history is collected."
    };

  }


  return {
    level: "low",
    label: "Low",
    detail:
      "Not enough historical information yet."
  };

}
