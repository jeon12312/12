function doGet() {
  const ss = SpreadsheetApp.openById('1Q8Qt1o_vXNbg3tSErkQ_KQcF_Ka2cJkSesYon45jBsQ');
  const sheet = ss.getSheetByName('abc');
  const range = sheet.getRange('A1:E3');
  const values = range.getValues();

  const weekdays = ['일', '월', '화', '수', '목', '금', '토'];

  function formatDate(value) {
    if (Object.prototype.toString.call(value) === '[object Date]') {
      const month = value.getMonth() + 1;
      const day = value.getDate();
      const dayName = weekdays[value.getDay()];
      return `${month}. ${day}.(${dayName})`;
    }
    return value;
  }

  let html = `
    <html>
    <head>
      <style>
        body {
          margin: 0;
          padding: 0;
          background-color: white;
          display: flex;
          justify-content: center;
        }
        table {
          border-collapse: collapse;
          margin-top: 0;
          background-color: #e5e5e5;
        }
        td {
          border: 1px solid #0a2b5a;
          padding: 5px 10px;
          text-align: center;
          min-width: 85px;
          height: 20px;
          font-size: 14px;
          font-weight: bold;
          color: #0a2b5a;
        }
      </style>
    </head>
    <body>
      <table>
  `;

  values.forEach(row => {
    html += '<tr>';
    row.forEach(cell => {
      html += `<td>${formatDate(cell)}</td>`;
    });
    html += '</tr>';
  });

  html += `
      </table>
    </body>
    </html>
  `;

  return HtmlService.createHtmlOutput(html);
}
