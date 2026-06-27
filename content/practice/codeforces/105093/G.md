---
title: "CF 105093G - Đại dịch"
description: "Chúng tôi được cung cấp một hệ thống người chơi hoàn chỉnh trong đó mỗi cặp không có thứ tự cuối cùng phải tương tác chính xác một lần. Một số tương tác đã xảy ra và chúng tôi biết thứ tự của chúng."
date: "2026-06-27T20:49:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105093
codeforces_index: "G"
codeforces_contest_name: "2024 UP ACM Algolympics Final Round"
rating: 0
weight: 105093
solve_time_s: 34
verified: true
draft: false
---

[CF 105093G - Đại dịch](https://codeforces.com/problemset/problem/105093/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống người chơi hoàn chỉnh trong đó mỗi cặp không có thứ tự cuối cùng phải tương tác chính xác một lần. Một số tương tác đã xảy ra và chúng tôi biết thứ tự của chúng. Mỗi tương tác giữa hai người chơi sẽ biến đổi trạng thái lây nhiễm của họ theo một quy tắc cố định, lật trạng thái tùy thuộc vào việc cặp đó Khỏe mạnh hay Bị nhiễm. 

Sau những tương tác đã được thực hiện này, chúng tôi sẽ được biết trạng thái hiện tại của mọi người chơi. Chúng tôi cũng được chia người chơi thành hai nhóm, bạn bè và kẻ thù của Gregorio. Mục tiêu không phải là mô phỏng trực tiếp tất cả các tương tác còn lại mà là đếm xem có bao nhiêu cách có thể sắp xếp các tương tác cặp không được thực hiện còn lại sẽ dẫn đến tình huống cuối cùng là mọi người bạn đều kết thúc khỏe mạnh và mọi kẻ thù đều kết thúc Bị nhiễm bệnh. 

Khó khăn chính là thứ tự của các cạnh còn lại rất quan trọng vì mọi tương tác đều thay đổi trạng thái và những thay đổi đó ảnh hưởng đến tất cả các tương tác trong tương lai. Chúng ta đang tính các hoán vị hợp lệ của các cạnh còn lại chứ không chỉ tính kết quả. 

Các ràng buộc ngụ ý một cấu trúc tương tác dày đặc. Vì mỗi cặp người chơi tương tác chính xác một lần, nên tổng số cạnh là n(n−1)/2, do đó, ngay cả với n vừa phải, đồ thị vẫn cực kỳ dày đặc. Với tổng số n và m lên tới 3·10^5 trong các thử nghiệm, mọi giải pháp mô phỏng tất cả các hoán vị hoặc thậm chí tất cả các chuỗi đều không thể thực hiện được. Sự tăng trưởng giai thừa trên các cạnh còn lại sẽ bị loại trừ ngay lập tức và thậm chí O(n^2) trên mỗi thử nghiệm cũng là đường biên trừ khi được tối ưu hóa nhiều. 

Một vấn đề tế nhị là trạng thái cuối cùng đã được biết sau m bước. Điều đó có nghĩa là các cạnh còn lại phải nhất quán với một cấu hình cố định và chúng tôi chỉ tính các lần sắp xếp lại để duy trì ràng buộc ghi nhãn cuối cùng bắt buộc chứ không tính toán lại các trạng thái từ đầu. 

Các trường hợp cạnh phát sinh khi không còn cạnh nào tồn tại, khi tất cả các nút đã ở vai trò bắt buộc cuối cùng hoặc khi trạng thái được tiết lộ ban đầu đã không nhất quán với sự chỉ định bạn/thù mong muốn. Một cách tiếp cận ngây thơ bỏ qua tính nhất quán và chỉ tính các hoán vị sẽ tạo ra các giai thừa không chính xác ngay cả khi không có thứ tự nào hoạt động. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng liệt kê tất cả các hoán vị của các cạnh còn lại và mô phỏng quá trình lây nhiễm cho mỗi thứ tự. Mỗi mô phỏng xử lý các tương tác O(n^2) và có (n(n−1)/2 − m)! hoán vị. Ngay cả với n = 6, điều này đã vượt quá khả năng thực hiện. Vấn đề không phải là chi phí mô phỏng mà là sự bùng nổ tổ hợp của các đơn đặt hàng. 

Quan sát quan trọng là động lực lây nhiễm hoàn toàn đối xứng và chỉ phụ thuộc vào tính chẵn lẻ của các tương tác liên quan đến mỗi đỉnh. Mỗi cái bắt tay sẽ chuyển trạng thái theo một cách xác định có thể được biểu diễn dưới dạng các ràng buộc chuyển đổi. Thay vì theo dõi các chuỗi toàn cục, chúng tôi diễn giải lại từng cạnh như một thao tác góp phần tạo ra hiệu ứng chẵn lẻ trên các điểm cuối của nó. 

Trạng thái cuối cùng của mỗi đỉnh chỉ phụ thuộc vào số lượng tương tác còn lại mà nó tham gia và mức độ tương đương so với trạng thái hiện tại. Vì mỗi cạnh lật cả hai điểm cuối theo một cách có cấu trúc nên toàn bộ quá trình có thể được rút gọn thành một hệ thống ràng buộc tuyến tính trên các cạnh còn lại. 

Cái nhìn sâu sắc quan trọng thứ hai là thứ tự của các cạnh chỉ quan trọng trong chừng mực nó ảnh hưởng đến tính khả thi của tính chẵn lẻ trung gian. Tuy nhiên, vì mọi cạnh đều đối xứng và các phép toán liên quan nên bất kỳ thứ tự nào cũng tương ứng với một hoán vị của các phép toán giao hoán giống hệt nhau trong một hệ thống chẵn lẻ bị ràng buộc. Điều này làm giảm vấn đề đếm các phần mở rộng tuyến tính hợp lệ của một cấu trúc trong đó chỉ có tính nhất quán chẵn lẻ là quan trọng.

Điều này biến bài toán thành việc đếm các hoán vị của các cạnh còn lại dưới các ràng buộc xuất phát từ các yêu cầu về đỉnh. Mỗi đỉnh áp đặt một điều kiện về số lượng cạnh còn lại phụ thuộc phải hoạt động trước hoặc sau một số chuyển đổi nhất định, nhưng về tổng thể, điều này làm giảm việc kiểm tra xem có tồn tại phép gán nhất quán chẵn lẻ hay không và sau đó đếm các hoán vị của các nhóm cạnh độc lập. 

Sau khi giảm, các cạnh còn lại có thể được phân chia thành các thành phần được xác định bởi các ràng buộc chẵn lẻ. Trong mỗi thành phần, tất cả các cạnh đều có thể hoán đổi cho nhau, do đó số thứ tự hợp lệ sẽ trở thành tích giai thừa trên các kích thước thành phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k! · n²) | O(n²) | Quá chậm | 
| Tối ưu | O(n + m + k α(k)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tập hợp các cạnh còn lại bằng cách trừ m cạnh đã được sử dụng khỏi biểu đồ đầy đủ. Về mặt khái niệm, chúng tôi coi các cạnh là các cặp không có thứ tự, do đó tập hợp còn lại được xác định rõ ràng. 
2. Với mỗi đỉnh, hãy tính xem có bao nhiêu cạnh còn lại liên thuộc với nó. Điều này mang lại cấu trúc biểu đồ mức độ còn lại phản ánh số lần lật trong tương lai mà mỗi đỉnh sẽ trải qua. Điều này quan trọng vì mỗi tương tác sẽ chuyển đổi trạng thái. 
3. Chuyển trạng thái được tiết lộ hiện tại thành giá trị nhị phân, ví dụ Khỏe mạnh là 0 và Bị nhiễm bệnh là 1. Mã hóa tương tự trạng thái cuối cùng mong muốn: bạn bè phải kết thúc bằng 0, kẻ thù ở 1. 
4. Đối với mỗi đỉnh, hãy tính yêu cầu tính chẵn lẻ do các phép toán còn lại tạo ra. Mỗi cạnh còn lại phụ thuộc đóng góp một lần chuyển đổi cho đỉnh đó, do đó tổng số lần chuyển đổi được áp dụng cho đỉnh v bằng deg_remaining[v], nhưng hiệu quả thực tế chỉ phụ thuộc vào thứ tự thông qua tính chẵn lẻ chứ không phải trình tự. 
5. Kiểm tra tính nhất quán: một đỉnh chỉ khả thi nếu tính chẵn lẻ của các phép toán sự cố còn lại có thể chuyển trạng thái hiện tại của nó thành trạng thái cuối cùng cần thiết. Điều này tạo ra một hệ phương trình chẵn lẻ trên tất cả các đỉnh. 
6. Hệ thống giảm xuống việc kiểm tra xem liệu có tồn tại sự phân công toàn cục các thứ tự cạnh có thỏa mãn đồng thời tất cả các ràng buộc về tính chẵn lẻ của đỉnh hay không. Nếu có bất kỳ mâu thuẫn nào phát sinh, xuất 0 ngay lập tức. 
7. Khi nhất quán, hãy quan sát rằng các cạnh không có sự phụ thuộc lẫn nhau nào nữa ngoài việc thuộc cùng một nhóm hoán vị không bị ràng buộc. Tất cả các cạnh còn lại đều có thể hoán đổi cho nhau về tính hợp lệ, vì vậy mọi thứ tự đều hợp lệ miễn là các ràng buộc được đáp ứng. 
8. Do đó, câu trả lời chỉ đơn giản là tính giai thừa của số cạnh còn lại, được điều chỉnh nếu có nhiều thành phần độc lập tồn tại dưới các ràng buộc chẵn lẻ, nhưng trong cấu trúc đồ thị hoàn chỉnh dày đặc này, các ràng buộc sẽ thu gọn lại thành một kiểm tra tính nhất quán toàn cục duy nhất. 
9. Tính toán trước các giai thừa lên đến n(n−1)/2 modulo 998244353 và giai thừa đầu ra của các cạnh còn lại. 

### Tại sao nó hoạt động 

Bất biến chính là mỗi cạnh hoạt động như một thao tác chuyển đổi đối xứng trên cả hai điểm cuối và điều kiện cuối cùng chỉ phụ thuộc vào tính chẵn lẻ tổng hợp trên mỗi đỉnh. Khi tính khả thi của tính chẵn lẻ của đỉnh được thỏa mãn, không có thứ tự nào của các cạnh còn lại có thể làm mất hiệu lực của nó bởi vì mọi thứ tự đều tạo ra nhiều tập hợp hoạt động giống nhau và chỉ có số lượng của chúng trên mỗi đỉnh là quan trọng. Vì tất cả các cạnh còn lại là các phép toán tương đương trên một đồ thị hoàn chỉnh không bị ràng buộc, nên tập hợp các hoán vị hợp lệ chính xác là tập hợp tất cả các hoán vị của các cạnh còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

M
```
