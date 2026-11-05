---
title: "CF 1190C - Tokitsukaze và Đấu tay đôi"
description: "Chúng ta được cấp một chuỗi nhị phân đại diện cho một dòng quân bài, mỗi quân bài hiển thị 0 hoặc 1. Một nước đi bao gồm việc chọn chính xác k vị trí liên tiếp và buộc tất cả chúng trở nên giống hệt nhau, toàn bộ là 0 hoặc toàn bộ là 1. Phần còn lại của mảng không thay đổi."
date: "2026-06-13T13:05:30+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1190
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 573 (Div. 1)"
rating: 2300
weight: 1190
solve_time_s: 260
verified: true
draft: false
---

[CF 1190C - Tokitsukaze và Đấu tay đôi](https://codeforces.com/problemset/problem/1190/C) 

**Đánh giá:** 2300 
**Tags:** bạo lực, trò chơi, tham lam 
**Thời gian giải:** 4 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân đại diện cho một dòng quân bài, mỗi quân bài hiển thị 0 hoặc 1. Một nước đi bao gồm việc chọn chính xác k vị trí liên tiếp và buộc tất cả chúng trở nên giống hệt nhau, toàn bộ là 0 hoặc toàn bộ là 1. Phần còn lại của mảng không thay đổi. Hai người chơi luân phiên di chuyển, bắt đầu với Tokitsukaze. Ai thực hiện một nước đi khiến toàn bộ mảng trở nên đồng nhất ngay lập tức sẽ thắng trò chơi đó. 

Khía cạnh quan trọng là việc di chuyển không phải là lật các bit riêng lẻ mà là sơn lại một đoạn liền kề. Người chơi được cho là sẽ chơi một cách tối ưu nên mọi nước đi đều được lựa chọn để tối đa hóa cơ hội chiến thắng của mình và giảm thiểu cơ hội chiến thắng của đối thủ. 

Nhiệm vụ là xác định xem Tokitsukaze thắng, Quailty thắng hay trận đấu có thể tiếp tục vô thời hạn (chính thức là hơn 10^9 nước đi, được coi là hòa). 

Ràng buộc n 10^5 ngay lập tức loại trừ bất kỳ mô phỏng nào của cây trò chơi. Mỗi lần di chuyển phân nhánh theo các khả năng O(nk) cho các phân đoạn và hai lựa chọn màu sắc, và thậm chí một tìm kiếm nông cũng trở nên theo cấp số nhân. Do đó, cấu trúc của bài toán phải suy luận về các thuộc tính tổng thể của cấu hình ban đầu. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi đã gần như đồng nhất ngoại trừ một cụm nhỏ các bit đối diện. Ví dụ: nếu tất cả các số 1 đều nằm trong một đoạn có độ dài k thì Tokitsukaze có thể vẽ lại đoạn đó và giành chiến thắng ngay lập tức. Một chiến lược ngây thơ chỉ xem xét màu sắc đa số hoặc các mẫu cục bộ sẽ thất bại ở đây vì chiến thắng phụ thuộc vào việc màu sắc thiểu số có thể nén được về mặt không gian chứ không phụ thuộc vào số lượng của nó. 

Một trường hợp lợi thế khác phát sinh khi không người chơi nào có thể buộc phải hoàn thành ngay lập tức, nhưng bàn cờ có thể được giữ ở trạng thái mà cả hai người chơi luôn có sẵn một nước đi không thắng. Điều này tạo ra kết quả "một lần nữa" và thường xảy ra khi cấu hình quá "dàn trải" để bất kỳ lần sơn lại k-window nào có thể thu gọn nó theo một số bước giới hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng trò chơi một cách rõ ràng. Từ một trạng thái nhất định, chúng tôi thử tất cả các lựa chọn O(n) của phân đoạn k và cả hai tùy chọn sơn lại. Sau mỗi lần di chuyển, chúng tôi kiểm tra xem bảng có đồng nhất hay không và tiếp tục đệ quy. Điều này mô hình chính xác cách chơi tối ưu, nhưng hệ số phân nhánh đã là Θ(n) và độ sâu cũng có thể là Θ(n), khiến cho sự bùng nổ không gian trạng thái ngay lập tức đối với n lên đến 10^5. 

Sự đơn giản hóa chính là ngừng suy nghĩ về trình tự di chuyển và thay vào đó suy luận về _khả năng nén của từng khối màu_. Một nước đi chỉ có thể loại bỏ toàn bộ một màu nếu màu đó nằm hoàn toàn bên trong một đoạn đã chọn. Điều này làm giảm vấn đề trong việc theo dõi các lần xuất hiện ngoài cùng bên trái và ngoài cùng bên phải của 0 và 1. 

Đặt số 1 chiếm một khoảng [l1, r1] và số 0 chiếm [l0, r0]. Cách duy nhất để hoàn thành trong một lần di chuyển là chọn phân đoạn k bao gồm tất cả các lần xuất hiện của một màu, sau đó sơn lại thành màu đối diện. Điều này ngay lập tức mang lại điều kiện thắng về mặt độ dài khoảng thời gian. 

Nếu không có màu nào có thể được bao phủ hoàn toàn bởi cửa sổ k, trò chơi sẽ xoay quanh việc liệu người chơi có thể buộc người kia rơi vào cấu hình thua hay không. Quan sát quan trọng là trò chơi trở nên vô hạn một cách chính xác khi không có nước đi nào có thể giảm thiểu nghiêm ngặt “khoảng không thể giảm” của cả hai màu theo cách quyết định, nghĩa là mọi nước đi đều có thể được trả lời theo cách khôi phục cấu hình phức tạp tương tự. Điều này xảy ra khi cả hai màu quá dàn trải so với k, do đó không người chơi nào có thể tách biệt một khối màu đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Nén khoảng thời gian (tối ưu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Quét chuỗi và ghi lại lần xuất hiện đầu tiên và cuối cùng của cả 0 và 1. Điều này cho ra hai khoảng [l0, r0] và [l1, r1]. Bước này nén toàn bộ trạng thái trò chơi thành thông tin có kích thước không đổi vì chỉ những vị trí cực đoan mới quan trọng để loại bỏ hoàn toàn. 
2. Tính độ dài của mỗi khoảng: len0 = r0 − l0 + 1 và len1 = r1 − l1 + 1. Chúng biểu thị khoảng cách lan truyền của mỗi màu. 
3. Kiểm tra xem Tokitsukaze có thể thắng chỉ trong một nước đi hay không. Điều này xảy ra nếu tồn tại một đoạn có độ dài k bao gồm tất cả các lần xuất hiện của 0 hoặc 1. Về mặt khoảng thời gian, điều này tương đương với việc kiểm tra xem min(len0, len1) ≤ k. Nếu vậy, Tokitsukaze chọn đoạn phủ màu đó và sơn lại, ngay lập tức giành chiến thắng. 
4. Nếu không thể giành chiến thắng ngay lập tức, hãy xác định xem liệu trò chơi có thể tiếp tục vô thời hạn hay không. Điều này xảy ra khi không người chơi nào có thể buộc trò chơi vào một vị trí mà một phân đoạn k có thể cô lập toàn bộ một màu và bất kỳ động thái nào cũng có thể bị đối thủ phản công một cách đối xứng. 

Điều kiện cấu trúc đặc trưng cho điều này là cả hai màu đều “quá rộng” so với k và chồng lên nhau theo cách mà mọi nỗ lực nén một bên đều để lại đủ cấu trúc để đối thủ khôi phục lại độ phức tạp. Trong chế độ này, trò chơi không bao giờ chuyển sang trạng thái đồng nhất trong thời gian giới hạn. 

1. Nếu không có chiến thắng ngay lập tức hoặc điều kiện chơi vô hạn, thì người chơi thứ hai sẽ buộc phải thắng bằng cách đáp lại mọi nước đi của Tokitsukaze bằng một nước đi kết thúc trong một bước. 

### Tại sao nó hoạt động 

Điều bất biến là tiến trình có ý nghĩa duy nhất trong trò chơi là giảm khoảng thời gian tối thiểu chứa tất cả các lần xuất hiện của một màu. Một nước đi sẽ loại bỏ hoàn toàn một màu (thắng ngay lập tức) hoặc không giảm đủ cả hai khoảng thời gian. Khi cả hai khoảng vượt quá k và được xen kẽ theo cách ngăn chặn sự cô lập, mọi chuyển động đều bảo tồn khả năng đảo ngược, dẫn đến một chu kỳ vô tận. Nếu tính đối xứng cấu trúc đó bị phá vỡ, người chơi thứ hai luôn có thể buộc cấu hình trở lại vị trí mà Tokitsukaze không thể tạo ra lực nén chiến thắng. 

## Giải pháp Python```
PythonRun
```Việc triển khai nén trạng thái vào các vị trí cực đoan của cả hai màu. Điều đó là đủ vì bất kỳ nước đi chiến thắng nào cũng phải loại bỏ toàn bộ khối màu, điều này chỉ có thể xảy ra nếu khối đó nằm hoàn toàn bên trong một phân đoạn đã chọn. Kiểm tra thắng ngay lập tức mã hóa trực tiếp điều kiện này. 

Logic còn lại phân biệt trường hợp bệnh lý trong đó cả hai màu được phân bố rộng rãi đến mức không có phân đoạn k nào có thể cô lập cấu trúc theo cách buộc phải chấm dứt. Séc`k * 2 <= n`đảm bảo có đủ chỗ cho các bước di chuyển không gây cản trở lặp đi lặp lại và điều kiện chồng lấp ranh giới đảm bảo cả hai màu vẫn tồn tại trên vùng có thể chơi được sau bất kỳ sửa đổi cục bộ nào. 

Phải cẩn thận với các chỉ số, vì giải pháp sử dụng chỉ mục dựa trên 0 trong khi lý luận về các khoảng mặc nhiên giả định các phạm vi bao gồm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```| Bước | l0,r0 | l1,r1 | len0 | len1 | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| quét lần đầu | 0,2 | 1,3 | 3 | 3 | cả hai nhịp > k | 
| kiểm tra chiến thắng ngay lập tức | - | - | - | - | sai | 
| kiểm tra vô hạn | chồng chéo không đủ | | | | sai | 
| kết quả | - | - | - | - | chất lượng | 

Cấu hình này có cả hai màu trải rộng và không có đoạn có độ dài-2 nào chứa tất cả các lần xuất hiện của một trong hai màu. 

### Ví dụ 2 

đầu vào:```

```| Bước | l0,r0 | l1,r1 | len0 | len1 | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| quét lần đầu | 3,4 | 0,2 | 2 | 3 | len0 ≤ k | 
| thắng ngay lập tức | vâng | | | | Tokitsukaze thắng | 

Ở đây tất cả các số 0 đều được chứa trong một đoạn có độ dài 2, vì vậy Tokitsukaze có thể vẽ lại đoạn đó thành 1 và hoàn thành ngay lập tức. 

Hai trường hợp này nêu bật sự khác biệt giữa cấu trúc thiểu số có thể nén được và cấu hình trải rộng trên toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một lần để tính toán các vị trí cực trị | 
| Không gian | O(1) | Chỉ có bốn chỉ số được lưu trữ | 

Giải pháp tối ưu cho n lên tới 10^5 vì nó giảm trò chơi xuống mức suy luận khoảng thời gian có kích thước không đổi, tránh mọi mô phỏng nước đi. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | tokitsukaze | đã xử lý thống nhất | 
| khối nén | tokitsukaze | khoảng ≤ k điều kiện | 
| xen kẽ | chất lượng | trường hợp thất bại dàn trải | 
| k = n | tokitsukaze | trường hợp sơn lại toàn bộ cạnh | 

## Vỏ cạnh 

Khi chuỗi đã đồng nhất, cả hai khoảng đều thu gọn về một điểm. Điều kiện thắng ngay lập tức được kích hoạt vì min(len0, len1) trở thành 0, phản ánh rằng không cần thực hiện hành động nào để đạt được tính đồng nhất. 

Khi k bằng n, bất kỳ di chuyển nào cũng sẽ ghi đè lên toàn bộ mảng. Thuật toán phân loại chính xác đây là vị trí chiến thắng vì người chơi đầu tiên có thể trực tiếp tạo ra sự đồng nhất bất kể cấu trúc ban đầu. 

Khi một màu xuất hiện trong một khối liền kề ngắn hơn k, khối đó hoàn toàn có thể bị loại bỏ trong một lần di chuyển. Việc nén khoảng thời gian nắm bắt chính xác điều này và điều kiện min(len0, len1) ≤ k đảm bảo phát hiện chính xác. 

Khi cả hai màu đều có độ xen kẽ cao, cả hai khoảng đều trải dài gần như toàn bộ mảng và không thể tách rời cả hai màu. Thuật toán định tuyến điều này vào nhánh không kết thúc hoặc nhánh thua tùy thuộc vào việc liệu sự chồng chéo ranh giới có cho phép chơi liên tục hay không, phản ánh việc không thể giảm độ phức tạp của cấu trúc chỉ trong một lần di chuyển.
