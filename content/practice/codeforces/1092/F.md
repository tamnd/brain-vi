---
title: "CF 1092F - Cây có chi phí tối đa"
description: "Chúng ta được cấp một cây có trọng số, trong đó mỗi nút mang một giá trị dương. Chúng tôi được phép chọn bất kỳ nút nào làm điểm tham chiếu và đối với lựa chọn đó, chúng tôi tính điểm được xác định bằng tổng giá trị của tất cả các nút nhân với khoảng cách của chúng đến nút đã chọn."
date: "2026-06-13T04:41:27+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1092
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 527 (Div. 3)"
rating: 1900
weight: 1092
solve_time_s: 467
verified: false
draft: false
---

[CF 1092F - Cây có chi phí tối đa](https://codeforces.com/problemset/problem/1092/F) 

**Xếp hạng:** 1900 
**Thẻ:** dfs và tương tự, dp, cây cối 
**Thời gian giải:** 7 phút 47 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có trọng số, trong đó mỗi nút mang một giá trị dương. Chúng tôi được phép chọn bất kỳ nút nào làm điểm tham chiếu và đối với lựa chọn đó, chúng tôi tính điểm được xác định bằng tổng giá trị của tất cả các nút nhân với khoảng cách của chúng đến nút đã chọn. Nhiệm vụ là tìm nút tối đa hóa số điểm này. 

Cấu trúc cây là tĩnh nên quyết định duy nhất là chọn gốc. Sau khi một nút gốc được cố định, mọi nút khác sẽ đóng góp tương ứng với khoảng cách nó nằm cách nút gốc đó, được chia tỷ lệ theo trọng lượng của nó. 

Các ràng buộc rất lớn, lên tới 200.000 nút. Bất kỳ giải pháp nào tính toán lại khoảng cách từ mọi gốc có thể một cách độc lập sẽ yêu cầu chạy truyền tải trên mỗi nút, dẫn đến khoảng$O(n^2)$hành vi trong cây có cấu trúc tuyến tính. Điều đó vượt xa những gì vừa vặn trong hai giây. Ngay cả việc tính toán khoảng cách cho tất cả các cặp cũng không thể thực hiện được, vì vậy giải pháp phải sử dụng lại các tính toán từng phần trên các nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi cây chỉ có một nút. Câu trả lời nhất thiết phải bằng 0 vì không có khoảng cách nào để tính tổng. Một trường hợp quan trọng khác là cây có hình ngôi sao, trong đó gốc tối ưu không phải lúc nào cũng là nút trung tâm. Nếu các nút có trọng số cao nằm ở các lá, việc dịch chuyển nút gốc về phía chúng sẽ làm tăng sự đóng góp của chúng nhiều hơn là làm tăng chi phí của các nút khác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi nút làm nút gốc. Đối với mỗi gốc ứng cử viên, chúng tôi chạy DFS hoặc BFS để tính khoảng cách đến tất cả các nút và tích lũy tổng có trọng số. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Giá mỗi gốc là$O(n)$, cho$O(n^2)$tổng thể. Với$n = 2 \cdot 10^5$, điều này là không thể thực hiện được. 

Quan sát quan trọng là khi chúng ta di chuyển gốc qua một cạnh, chỉ có khoảng cách tương đối mới thay đổi theo cách có cấu trúc. Nếu chúng ta biết chi phí cho một gốc, chúng ta có thể cập nhật chi phí cho hàng xóm của nó mà không cần tính toán lại mọi thứ. Sự thay đổi chỉ phụ thuộc vào tổng trọng lượng của các nút trong cây con được di chuyển đến gần hoặc xa hơn. 

Điều này dẫn đến một ý tưởng tái tạo lại cây cổ điển. Trước tiên, hãy tính tổng trọng lượng của cây và chi phí cho một gốc tùy ý, chẳng hạn như nút 1. Sau đó, đối với mỗi cạnh từ cha đến con, chúng ta rút ra chi phí thay đổi như thế nào khi chuyển gốc từ cha sang con. Các nút trong cây con của con tiến gần hơn một nút, trong khi tất cả các nút khác tiến xa hơn một nút. Điều này cho phép một$O(1)$chuyển tiếp trên mỗi cạnh sau một lần xử lý trước DFS. 

Chúng tôi tính toán tổng trọng số của cây con để thực hiện các chuyển đổi này. Khi chúng ta có trọng số của cây con, việc root lại sẽ trở thành một quá trình truyền tuyến tính trên cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Root lại DP |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định$S$là tổng của tất cả các trọng số nút. Chúng ta root cây tại một nút tùy ý, tính tổng các cây con và tính chi phí của cấu hình gốc này. Sau đó, chúng tôi truyền chi phí này đến tất cả các nút khác bằng cách khởi động lại. 

1. Chọn một nút tùy ý, thường là nút 1, làm gốc ban đầu. Điều này cho chúng ta một góc nhìn cố định để tính toán tất cả thông tin về cây con một cách nhất quán. 
2. Chạy DFS để tính toán hai giá trị cho mỗi nút: tổng trọng số của cây con và đóng góp ban đầu của nó vào tổng chi phí từ gốc đã chọn. Tổng cây con là cần thiết để hiểu chi phí thay đổi như thế nào khi gốc dịch chuyển. 
3. Trong DFS, tính chi phí ban đầu ở gốc bằng cách tích lũy$depth[node] \cdot a[node]$. Điều này thiết lập một đường cơ sở chính xác cho việc root lại. 
4. Lưu trữ tổng trọng số của tất cả các nút. Giá trị này xác định mức độ thay đổi chi phí khi một nút di chuyển một bước gần hơn hoặc xa hơn khỏi gốc. 
5. Thực hiện DFS thứ hai để root lại. Giả sử chúng ta đang ở một nút$u$với chi phí đã biết và chúng tôi chuyển phần gốc sang phần tử con$v$. 
6. Khi di chuyển từ$u$ĐẾN$v$, tất cả các nút trong$v$cây con của trở nên gần hơn một bước tới gốc. Tổng trọng lượng của chúng là$subtree[v]$, do đó đóng góp của họ giảm đi$subtree[v]$. 
7. Tất cả các nút bên ngoài$v$cây con của trở nên xa hơn một bước. Tổng trọng lượng của chúng là$S - subtree[v]$, do đó đóng góp của họ tăng thêm$S - subtree[v]$. 
8. Kết hợp cả hai hiệu ứng để tính:$$cost[v] = cost[u] + (S - subtree[v]) - subtree[v]$$Điều này mang lại sự chuyển tiếp theo thời gian không đổi. 
9. Theo dõi giá trị chi phí tối đa trên tất cả các nút trong quá trình truyền tải. 

### Tại sao nó hoạt động 

Công thức root lại dựa trên thực tế là việc di chuyển gốc qua một cạnh sẽ thay đổi mỗi khoảng cách đúng một đơn vị, tăng hoặc giảm tùy thuộc vào thành viên của cây con. Việc phân vùng thành các nút cây con và không phải cây con đảm bảo mỗi thay đổi khoảng cách của nút được tính chính xác một lần. Vì tổng của cây con mô tả đầy đủ tổng phân bố trọng lượng nên việc chuyển đổi chi phí là chính xác và không có thông tin nào bị mất trong quá trình chuyển đổi. 

## Giải pháp Python```
PythonRun
```DFS đầu tiên xây dựng tổng cây con và tính toán chi phí ban đầu từ gốc. DFS thứ hai thực hiện việc root lại, cập nhật chi phí theo thời gian không đổi trên mỗi cạnh bằng công thức dẫn xuất. Việc lưu và khôi phục tạm thời của`cost0`đảm bảo việc truyền chính xác xuống các nhánh khác nhau mà không cần tính toán lại. 

Một cạm bẫy triển khai phổ biến là quên rằng tổng của cây con chỉ được xác định đối với gốc ban đầu. Một cách khác là cập nhật không chính xác chi phí một cách đối xứng; bản cập nhật chính xác phụ thuộc hoàn toàn vào tư cách thành viên của cây con chứ không phải lý do khoảng cách chung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi root ở nút 1. 

| Bước | Nút | Tổng cây con | Chi phí | 
| --- | --- | --- | --- | 
| DFS1 | 1 | 6 | 0·1 + 1·2 + 2·3 = 8 | 
| Chuyển gốc sang 2 | 2 | 5 | 8 + (6-5) - 5 = 4 | 
| Chuyển gốc sang 3 | 3 | 3 | 4 + (6-3) - 3 = 4 | 

Tối đa là 8 tại nút 1. 

Điều này cho thấy việc dịch chuyển gốc thay đổi sự đóng góp hoàn toàn dựa trên trọng số của cây con như thế nào. 

### Ví dụ 2 

đầu vào:```

```Gốc tại nút 1: 

| Bước | Nút | Tổng cây con | Chi phí | 
| --- | --- | --- | --- | 
| DFS1 | 1 | 14 | 0 | 
| Chuyển đến 3 | 3 | 3 | 0 + (14-3) - 3 = 8 | 
| Chuyển đến 4 | 4 | 1 | 8 + (14-1) - 1 = 20 | 

Tối đa xảy ra ở nút 4 hoặc 5 tùy thuộc vào quá trình truyền tải. 

Điều này chứng tỏ rằng các rễ tối ưu có xu hướng nằm về phía các cây con nặng hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi DFS truy cập vào mỗi nút và cạnh một lần | 
| Không gian |$O(n)$| Danh sách kề và mảng phụ | 

Độ phức tạp tuyến tính là cần thiết vì kích thước đầu vào đạt tới 200.000 nút. Bất kỳ phép tính lại khoảng cách bậc hai nào cũng sẽ vượt quá cả giới hạn về thời gian và bộ nhớ, trong khi kỹ thuật khởi động lại đảm bảo mỗi cạnh chỉ đóng góp công việc không đổi. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | trường hợp cạnh cơ sở | 
| chuỗi | 20 | hành vi đường đi dài nhất | 
| ngôi sao | 16 | độ nhạy reroot | 
| trọng lượng đồng đều | 10 | tính đúng đắn đối xứng | 

## Vỏ cạnh 

Đối với một đầu vào nút duy nhất, DFS khởi tạo chi phí bằng 0 vì độ sâu bằng 0 và không tồn tại cạnh nào. Giai đoạn khởi động lại không kích hoạt bất kỳ chuyển đổi nào, do đó mức tối đa vẫn bằng 0, phù hợp với kết quả đầu ra mong đợi. 

Đối với cây hình ngôi sao trong đó một lá có trọng lượng lớn hơn đáng kể, DFS đầu tiên sẽ tính toán chi phí từ gốc ở giữa. Khi root lại lá nặng đó, kích thước cây con trở nên nhỏ, khiến công thức tăng chi phí mạnh do hầu hết các nút di chuyển xa hơn. Thuật toán nắm bắt chính xác điều này vì tổng cây con tách biệt chính xác nhánh nặng, đảm bảo công thức chuyển đổi được áp dụng rõ ràng.
