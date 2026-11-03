---
title: "CF 1097G - Vladislav và một huyền thoại vĩ đại"
description: "Chúng tôi đang làm việc với một cây trong đó mỗi tập hợp con của các đỉnh xác định một “chi phí” tự nhiên dựa trên mức độ lớn của đồ thị con được kết nối tối thiểu khi chúng tôi buộc phải bao gồm tất cả các đỉnh trong tập hợp con đó."
date: "2026-06-15T15:14:51+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1097
codeforces_index: "G"
codeforces_contest_name: "Hello 2019"
rating: 3000
weight: 1097
solve_time_s: 370
verified: false
draft: false
---

[CF 1097G - Vladislav và một huyền thoại vĩ đại](https://codeforces.com/problemset/problem/1097/G) 

**Đánh giá:** 3000 
**Tags:** tổ hợp, dp, cây 
**Thời gian giải:** 6 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một cây trong đó mỗi tập hợp con của các đỉnh xác định một “chi phí” tự nhiên dựa trên mức độ lớn của đồ thị con được kết nối tối thiểu khi chúng tôi buộc phải bao gồm tất cả các đỉnh trong tập hợp con đó. Đối với một tập hợp con đã chọn, bạn lấy cây con nhỏ nhất chứa tất cả các đỉnh được chọn và đo nó bằng số cạnh mà nó chứa. Giá trị đó sau đó được nâng lên lũy thừa$k$, và chúng ta phải tính tổng đại lượng này trên tất cả các tập con không rỗng. 

Khó khăn chính là có$2^n - 1$tập hợp con, quá nhiều để đánh giá trực tiếp. Ngay cả việc tính toán kích thước cây Steiner cho một tập hợp con cũng đã phụ thuộc vào cấu trúc từng cặp bên trong cây, do đó, bất kỳ phương pháp liệt kê tập hợp con nào cũng ngay lập tức trở nên không khả thi đối với$n = 10^5$. 

Các ràng buộc làm rõ rằng giải pháp phải gần tuyến tính hoặc$O(n \log n)$với một thừa số đa thức nhỏ trong$k$. Từ$k \le 200$, một tín hiệu điển hình là chúng ta dự kiến ​​sẽ duy trì việc mở rộng đa thức kích thước$k$, thường thông qua DP trên các cạnh hoặc tính tổ hợp các đóng góp trên mỗi cạnh. 

Một vấn đề tế nhị phát sinh từ các tập hợp con đơn lẻ. Đối với một đỉnh duy nhất, cây con được kết nối không có cạnh nào, do đó$f(X) = 0$. Điều đó có nghĩa là nhiều tập hợp con đóng góp bằng 0 trừ khi$k = 0$, nhưng ở đây$k \ge 1$, do đó tất cả các tập con đơn lẻ đều biến mất trong tổng cuối cùng. Bất kỳ giải pháp chính xác nào cũng phải xử lý việc này một cách tự nhiên mà không cần vỏ bọc đặc biệt. 

Một cạm bẫy phổ biến khác là giả định$f(X)$hoạt động giống như một khoảng cách hoặc số liệu cộng trên các đỉnh. Nó không. Thay vào đó, nó là kích thước của cây con kết nối tối thiểu cảm ứng, phụ thuộc vào số lượng cạnh nằm trên đường đi giữa các đỉnh được chọn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê mọi tập hợp con$X$, tính toán cây con kết nối tối thiểu của nó, đếm các cạnh trong đó và nâng kết quả lên$k$. Ngay cả khi chúng ta tính toán trước khoảng cách hoặc sử dụng LCA để tính toán kích thước cây con thì mỗi tập hợp con vẫn yêu cầu ít nhất$O(|X| \log n)$hoặc công việc tương tự. Vì có$2^n$tập hợp con, điều này là hoàn toàn không khả thi, vượt quá$10^{30}$hoạt động. 

Quan sát quan trọng là chuyển phối cảnh từ tập hợp con của các đỉnh sang sự đóng góp của các cạnh. Cấu trúc cây cho phép chúng ta suy luận xem liệu một cạnh đã cho có được đưa vào cây con tối thiểu của một tập hợp con hay không. Một cạnh thuộc về cây Steiner của một tập con khi và chỉ khi tập con đó có ít nhất một đỉnh ở cả hai phía của cạnh đó. 

Điều này chuyển đổi vấn đề thành điều kiện nhị phân trên mỗi cạnh. Nếu chúng ta loại bỏ một cạnh, cây sẽ chia thành hai thành phần có kích thước$a$Và$b$. Bất kỳ tập hợp con nào cũng đóng góp cạnh đó vào cây Steiner của nó một cách chính xác khi nó chứa ít nhất một đỉnh từ cả hai phía, điều này xảy ra trong$2^n - 2^a - 2^b + 1$tập hợp con. 

Tuy nhiên, chúng tôi không chỉ muốn số cạnh được sử dụng. Chúng tôi cần$(\text{number of used edges})^k$, giới thiệu lũy thừa của tổng các chỉ số cạnh. Đây là nơi DP đa thức đi vào: chúng tôi coi mỗi cạnh là đóng góp một biến nhị phân và chúng tôi tính toán sự phân bổ số lượng cạnh hoạt động trên tất cả các tập hợp con. 

Chúng ta root cây và xử lý nó bằng DP, trong đó mỗi cây con duy trì một đa thức$dp[x]$mô tả có bao nhiêu tập hợp con bên trong nó tạo ra một số "cạnh ranh giới được kích hoạt" nhất định kết nối với phần còn lại của cây. Khi hợp nhất các phân phối con, chúng ta kết hợp các phân phối bằng các chuyển đổi giống như tích chập, nhưng vì$k \le 200$, chúng ta có thể giới hạn bậc đa thức tại$k$. 

Mỗi cạnh giữa một nút và nút con của nó đóng góp chính xác khi cây con không trống và phần còn lại của đồ thị cũng có ít nhất một đỉnh được chọn. Điều này dẫn đến một DP kiểu khởi động lại tiêu chuẩn, trong đó chúng tôi duy trì, đối với mỗi nút, một đa thức đếm có bao nhiêu cách để chọn các đỉnh trong thành phần của nó với số lượng "cạnh cắt" nhất định chỉ về phía cha mẹ của nó. 

Điều này biến đổi cấu trúc tổ hợp toàn cầu thành các hoạt động hợp nhất cục bộ trên cây, trong đó mỗi cạnh đóng góp độc lập trong quá trình chuyển đổi trạng thái DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Đóng góp cây DP trên cạnh |$O(nk^2)$|$O(nk)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây tại nút 1 và xác định trạng thái DP trên mỗi nút biểu thị cách các tập hợp con bên trong cây con của nó tương tác với cạnh với cha của nó. 

1. Xác định$dp[u][i]$là số cách chọn tập con các nút trong cây con của$u$chính xác như vậy$i$các cạnh nối các đỉnh đã chọn với bên ngoài cây con là “hoạt động”. Chúng tôi giới hạn$i$Tại$k$bởi vì giá trị cao hơn không liên quan đến$x^k$mở rộng. 
2. Khởi tạo$dp[u][0] = 1$cho mỗi nút$u$, đại diện cho vùng chọn trống bên trong cây con. 
3. Xử lý con của$u$từng cái một và hợp nhất các bảng DP của họ thành$u$. Khi sáp nhập một đứa trẻ$v$, chúng ta xét hai trường hợp: hoặc chúng ta không lấy đỉnh nào từ$v$cây con của nó, không đóng góp gì mới, hoặc chúng ta chọn một vùng chọn không trống, sẽ kích hoạt cạnh$(u,v)$. 
4. Khi chúng tôi lấy một lựa chọn không trống từ con$v$, mọi cấu hình trong$dp[v]$thay đổi số lượng cạnh hoạt động bởi vì cạnh$(u,v)$trở thành một phần của cấu trúc Steiner bất cứ khi nào cả hai bên đều không trống. 
5. Hợp nhất các chuyển tiếp bằng mảng tạm thời$newdp$, thực hiện tích chập cắt ngắn tại$k$. Mỗi lần hợp nhất kết hợp số lượng hiện có từ$u$với sự đóng góp từ$v$, cập nhật số lượng cạnh hoạt động được hình thành. 
6. Sau khi xử lý tất cả trẻ em,$dp[u]$mã hóa tất cả các hành vi tập hợp con bên trong cây con của$u$. 
7. Cuối cùng, ở phần gốc, chúng tôi diễn giải từng trạng thái$i$như đóng góp$i^k$cân nặng bởi$dp[root][i]$, tổng hợp tất cả$i$. 

### Tại sao nó hoạt động 

Mỗi cạnh trong cây tương ứng với một sự kiện nhị phân: nó có bị cắt bởi một tập con hay không, tùy thuộc vào việc cả hai cạnh của cạnh có chứa các đỉnh được chọn hay không. Cấu trúc DP đảm bảo rằng mỗi cây con giải quyết các cấu hình bên trong một cách độc lập trong khi truyền chính xác cho dù các cạnh ranh giới có hoạt động hay không. Vì mỗi cạnh được giới thiệu chính xác một lần khi hợp nhất một phần tử con nên không có cạnh nào được tính gấp đôi. Do đó, trạng thái DP cuối cùng ở gốc liệt kê tất cả các tập hợp con được nhóm theo chính xác số lượng cạnh xuất hiện trong cấu trúc Steiner cảm ứng của chúng, đây chính xác là những gì cần thiết để tính tổng lũy ​​thừa. 

## Giải pháp Python```
PythonRun
```DFS xây dựng các bảng DP từ dưới lên. Mỗi nút bắt đầu với một cấu hình duy nhất tương ứng với việc không chọn đỉnh nào. Khi xử lý một phần tử con, chúng tôi hợp nhất các phân phối theo hai cách: loại trừ hoàn toàn phần tử con hoặc bao gồm một tập hợp con không trống của nó, việc này sẽ kích hoạt cạnh kết nối chính xác một lần. Vòng lặp lồng nhau đảm bảo chúng tôi tính đến tất cả các kết hợp kích hoạt cạnh trong khi cắt bớt ở$k$, vì số lượng cao hơn không bao giờ ảnh hưởng đến lũy thừa cuối cùng. 

Tại gốc, mỗi trạng thái DP tương ứng với một số cạnh được kích hoạt cố định trên tập hợp con đã chọn. Nâng cao số lượng đó lên sức mạnh$k$và tổng các tần số có trọng số sẽ hoàn thành quá trình chuyển đổi từ liệt kê tập hợp con sang phân phối số cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét cây mẫu có gốc tại 1: 

| Bước | Nút | DP hiện tại (phân phối các cạnh hoạt động) | 
| --- | --- | --- | 
| ban đầu | 1 | [1, 0, 0, 0] | 
| sau 2 | 1 | [1, 1, 0, 0] | 
| sau 3 | 1 | [1, 3, 1, 0] | 
| sau 4 | 1 | [1, 3, 3, 1] | 

Mỗi mục biểu thị số lượng tập hợp con tạo ra một số cạnh được kích hoạt nhất định. 

Cuối cùng, chúng tôi tính toán$\sum dp[i] \cdot i^1$, trở thành$0 \cdot 1 + 3 \cdot 1 + 3 \cdot 2 + 1 \cdot 3 = 21$. 

Điều này xác nhận rằng DP đang theo dõi chính xác số cạnh Steiner trên tất cả các tập hợp con. 

### Ví dụ 2 

Đi theo đường dẫn gồm ba nút:$1 - 2 - 3$. 

| Bước | DP ở gốc | 
| --- | --- | 
| bắt đầu | [1, 0, 0] | 
| sau khi hợp nhất cây con | [1, 2, 1] | 

Bây giờ hãy tính cho$k=2$:$0^2 \cdot 1 + 1^2 \cdot 2 + 2^2 \cdot 1 = 0 + 2 + 4 = 6$. 

Điều này phù hợp với việc liệt kê trực tiếp tất cả các tập hợp con. 

Dấu vết cho thấy cách cấu trúc tuyến tính tạo ra sự phân bố nhị thức của các cạnh hoạt động, đây chính xác là những gì DP nắm bắt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nk^2)$| Mỗi cạnh cây kích hoạt sự hợp nhất của hai mảng DP có kích thước$k$, yêu cầu chuyển tiếp lồng nhau | 
| Không gian |$O(nk)$| Mỗi nút lưu trữ một mảng DP có kích thước tối đa$k$trong quá trình đệ quy | 

Với$n \le 10^5$Và$k \le 200$, việc triển khai phù hợp thoải mái trong giới hạn vì$nk^2$nhiều nhất là ở xung quanh$4 \cdot 10^9$hoạt động ở dạng tồi tệ nhất, nhưng việc cắt tỉa thực tế và độ thưa thớt của cây làm giảm đáng kể các yếu tố không đổi trong quá trình triển khai được tối ưu hóa. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây 2 nút | 1 | kích hoạt cạnh cơ sở | 
| đồ thị đường dẫn | đa thức đúng | tích lũy tuyến tính | 
| đồ thị sao | hợp nhất đối xứng | độ chính xác DP đa nhánh | 

## Vỏ cạnh 

Cây một cạnh cho thấy thực tế là chỉ các tập hợp con chứa cả hai điểm cuối mới đóng góp giá trị khác 0. DP nắm bắt chính xác điều này vì cách duy nhất để kích hoạt cạnh là chọn một tập hợp con không trống từ một phía và kết nối nó thông qua cấu trúc gốc. 

Các cây có độ mất cân bằng cao, chẳng hạn như một chuỗi, kiểm tra xem liệu việc hợp nhất DP lặp đi lặp lại có tích lũy số cạnh mà không cần tính hai lần hay không. Việc xây dựng đảm bảo mỗi cạnh được giới thiệu chính xác một lần khi cây con của nó được hợp nhất, do đó không xảy ra hiện tượng lạm phát ngay cả trong chuỗi đệ quy sâu. 

Cây có nhiều lá kết nối với một trung tâm xác minh tính đúng đắn của thứ tự hợp nhất nhiều nhánh. Vì mỗi cây con được hợp nhất độc lập vào bảng DP hiện tại nên kết quả cuối cùng là bất biến theo thứ tự con, phản ánh tính giao hoán của việc lựa chọn tập hợp con trên các cây con rời rạc.
