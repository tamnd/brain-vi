---
title: "CF 105137E - Game hay"
description: "Cấu trúc này là một cây có gốc với nút 1 đóng vai trò là gốc, nhưng về mặt khái niệm, nó được vẽ lộn ngược để trọng lực đẩy các vật thể về phía gốc. Mỗi nút có thể chứa tối đa một quả bóng. Chúng tôi được cung cấp một chuỗi các nút bắt đầu và chúng tôi thả từng quả bóng một."
date: "2026-06-27T18:44:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105137
codeforces_index: "E"
codeforces_contest_name: "TheForces Round #30 (Good-Forces)"
rating: 0
weight: 105137
solve_time_s: 29
verified: false
draft: false
---

[CF 105137E - Trò chơi hay](https://codeforces.com/problemset/problem/105137/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cấu trúc này là một cây có gốc với nút 1 đóng vai trò là gốc, nhưng về mặt khái niệm, nó được vẽ lộn ngược để trọng lực đẩy các vật thể về phía gốc. Mỗi nút có thể chứa tối đa một quả bóng. 

Chúng tôi được cung cấp một chuỗi các nút bắt đầu và chúng tôi thả từng quả bóng một. Khi một quả bóng được thả vào một nút, nó sẽ cố gắng di chuyển dọc theo con đường duy nhất về phía gốc, luôn đi lên từng cạnh một. Quả bóng tiếp tục di chuyển miễn là nút tiếp theo trên đường đi đó còn trống. Ngay khi đến một nút đã bị chiếm dụng, nó sẽ dừng ở nút trống cuối cùng trước nó. Nếu ngay cả nút xuất phát đã bị chiếm giữ, quả bóng không thể được đặt và câu trả lời là -1 cho quả bóng đó. 

Do đó, đầu ra của mỗi quả bóng là nút cuối cùng nơi nó ổn định hoặc -1 nếu nó không bao giờ tìm thấy vị trí tự do. 

Các ràng buộc đủ lớn để cả số lượng nút và số lượng quả bóng có thể đạt tới một triệu cho mỗi trường hợp thử nghiệm, với tổng số tiền trên các trường hợp thử nghiệm lên tới hai triệu. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng từng quả bóng di chuyển từng bước dọc theo cây. Một chuỗi trường hợp xấu nhất có độ dài n với m quả bóng sẽ dẫn đến 10^12 thao tác nếu được mô phỏng một cách ngây thơ. 

DFS trực tiếp hoặc truyền tải đi lên lặp đi lặp lại cho mỗi truy vấn sẽ không thành công. Ngay cả việc truyền tải lân cận được tối ưu hóa tốt trên mỗi quả bóng cũng quá chậm vì mỗi quả bóng có thể đi qua một chuỗi dài. 

Trường hợp cạnh khó phát hiện khi nhiều quả bóng bắt đầu ở gần các lá của một chuỗi dài. 

Ví dụ: hãy xem xét chuỗi 1 - 2 - 3 - 4 - 5 (gốc 1) và các quả bóng đến 5, 5, 5, 5. Quả bóng đầu tiên đi 5 → 4 → 3 → 2 → 1, nhưng quả bóng thứ hai có thể dừng sớm hơn tùy thuộc vào tỷ lệ lấp đầy. Một mô phỏng ngây thơ không theo dõi "tổ tiên có sẵn tiếp theo" sẽ liên tục đi theo cùng một con đường và chuyển sang hành vi bậc hai. 

Một trường hợp khác là khi gốc bị chiếm dụng sớm. Sau đó, tất cả các quả bóng tiếp theo trên đường dẫn gốc đó phải trả về -1 ngay cả khi các nút thấp hơn vẫn tồn tại. 

## Phương pháp tiếp cận 

Mô phỏng lực mạnh xử lý từng quả bóng một cách độc lập. Đối với mỗi quả bóng, chúng tôi liên tục di chuyển từ nút bắt đầu đến nút gốc của nó cho đến khi chúng tôi đến được gốc hoặc tìm thấy nút trống. Chúng tôi cũng kiểm tra tỷ lệ lấp đầy ở mỗi bước. 

Điều này đúng vì nó tuân theo trực tiếp các quy luật chuyển động. Tuy nhiên, mỗi truy vấn có thể đi qua một đường dẫn có độ dài O(n) và có m truy vấn, dẫn đến hành vi O(nm) trong cây hình chuỗi. Với n, m lên tới 10^6 thì điều này là không thể. 

Quan sát quan trọng là mỗi nút chuyển đổi từ “miễn phí” sang “bị chiếm dụng” đúng một lần. Sau khi một nút bị chiếm giữ, các quả bóng trong tương lai không bao giờ cần phải xem xét nút đó nữa. Điều này cho phép chúng ta nén các đường đi lên lặp đi lặp lại bằng cách sử dụng cấu trúc tập hợp rời rạc trên cây. 

Thay vì liên tục đi lên trên, chúng tôi duy trì cho mỗi nút một con trỏ tới nút tổ tiên gần nhất vẫn còn trống. Khi một nút bị chiếm dụng, chúng tôi sẽ “hợp nhất nó” bằng cách chuyển hướng nó đến nút mẹ của nó. Sau đó, thao tác tìm sẽ chuyển trực tiếp đến nút có sẵn tiếp theo mà không cần quét các nút bị chiếm giữ trung gian. 

Điều này biến việc truyền tải đường dẫn lặp đi lặp lại thành thời gian khấu hao gần như không đổi cho mỗi thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu (DSU trên con trỏ gốc) | O((n + m) α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi nút là có sẵn ban đầu. Chúng tôi cũng tính toán trước nút cha của mỗi nút bằng cách sử dụng BFS hoặc DFS từ nút gốc. 

Sau đó, chúng tôi duy trì cấu trúc tập hợp rời rạc trong đó mỗi nút trỏ đến tổ tiên ứng cử viên tiếp theo.

1. Root cây tại nút 1 và tính toán các con trỏ cha cho mỗi nút. Điều này xác định đường di chuyển đi lên hợp lệ duy nhất cho mỗi nút. 
2. Khởi tạo một mảng DSU trong đó find(x) ban đầu trả về x. Điều này thể hiện rằng mọi nút hiện đều miễn phí. 
3. Xử lý bóng theo thứ tự. Với mỗi nút bắt đầu x, hãy tính v = find(x)
