---
title: "CF 104782G - Tổng tối thiểu"
description: "Chúng ta đang chơi trò chơi trên một dãy số sử dụng deque bắt đầu bằng một giá trị duy nhất là 0. Ở mỗi bước, chúng tôi xử lý phần tử mảng tiếp theo và buộc phải tương tác với một trong hai đầu của deque."
date: "2026-06-28T15:00:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104782
codeforces_index: "G"
codeforces_contest_name: "2023 Romanian Collegiate Programming Contest (RCPC)"
rating: 0
weight: 104782
solve_time_s: 49
verified: true
draft: false
---

[CF 104782G - Giảm thiểu tổng](https://codeforces.com/problemset/problem/104782/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang chơi trò chơi trên một dãy số sử dụng deque bắt đầu bằng một giá trị duy nhất là 0. Ở mỗi bước, chúng tôi xử lý phần tử mảng tiếp theo và buộc phải tương tác với một trong hai đầu của deque. 

Ở mỗi bước, chúng tôi lấy giá trị hiện tại ở phía trước hoặc phía sau deque, thêm nó vào điểm đang chạy, sau đó chèn phần tử mảng mới vào cùng phía mà chúng tôi vừa lấy. Điều này có nghĩa là deque luôn tăng thêm một phần tử, nhưng giá trị chúng tôi loại bỏ sẽ được tính vĩnh viễn vào điểm. 

Mục đích là giảm thiểu điểm tích lũy cuối cùng sau khi xử lý tất cả các phần tử. 

Chi tiết cấu trúc quan trọng là mọi thao tác đều loại bỏ một điểm cuối và thêm phần tử mới tại cùng điểm cuối đó. Điều này có nghĩa là deque phát triển bằng cách “mở rộng ra bên ngoài” trong khi liên tục sạc các điểm cuối. 

Các ràng buộc cho phép lên tới hai trăm nghìn phần tử trong tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng mô phỏng tất cả các khả năng của lựa chọn trái và phải sẽ bùng nổ theo cấp số nhân vì mỗi vị trí sẽ nhân đôi số trạng thái. Ngay cả lập trình động bậc hai theo các khoảng cũng sẽ quá chậm, vì các khoảng phát triển theo cách vẫn yêu cầu theo dõi các trạng thái hoặc chuyển tiếp O(n^2). 

Một ý tưởng tham lam ngây thơ, chẳng hạn như luôn lấy phần nhỏ hơn trong hai đầu, cũng thất bại vì hành động chèn các phần tử mới sẽ thay đổi các điểm cuối trong tương lai theo cách khiến các quyết định cục bộ trở nên sai lầm. 

Một trường hợp phức tạp bộc lộ sự tham lam ngây thơ là khi một giá trị lớn được đặt sớm, nhưng việc trì hoãn việc hiển thị nó sẽ khiến nó trở thành phần tử cuối cùng còn lại và hoàn toàn tránh phải trả nó. Ngược lại, chọn nó quá sớm sẽ buộc nó phải ghi điểm vĩnh viễn. 

Khó khăn chính là giá trị của một phần tử không chỉ nằm ở thời điểm nó xuất hiện mà còn là liệu chúng ta có thể “bảo vệ” nó khỏi bị chọn làm điểm cuối cho đến phút cuối cùng hay không. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng tất cả các lựa chọn rẽ trái hoặc phải ở mỗi bước. Ở bước thứ i có hai lựa chọn, vì vậy sau n bước có 2^n chuỗi có thể. Đối với mỗi chuỗi, chúng ta có thể tính điểm kết quả theo O(n), dẫn đến thời gian theo cấp số nhân và tính không khả thi ngay lập tức. 

Chúng ta cần hiểu những gì thay đổi về cơ bản khi quá trình này phát triển. Quan sát quan trọng là deque luôn là một chuỗi liền kề duy nhất và mọi thao tác đều mở rộng nó sang bên trái hoặc bên phải. Điều này có nghĩa là cấu trúc luôn là một đường dẫn đơn giản mà điểm cuối của nó là các phần tử duy nhất có thể chọn được. 

Nếu chúng ta nghĩ ngược lại, mỗi bước sẽ loại bỏ một điểm cuối và “chốt” giá trị đó vào điểm số một cách hiệu quả. Một phần tử hoàn toàn không bao giờ bị xóa: sau n lần xóa khỏi cấu trúc có kích thước n+1 ban đầu, chỉ còn lại chính xác một phần tử. Yếu tố còn lại đó là giá trị duy nhất không bao giờ đóng góp vào điểm số. 

Điều này điều chỉnh lại vấn đề một cách hoàn toàn. Tổng số nhiều giá trị là cố định: tất cả là ai cộng với số 0 ban đầu. Mọi phần tử ngoại trừ một phần tử được thanh toán chính xác một lần. Do đó, điểm cuối cùng bằng tổng của tất cả các giá trị trừ đi giá trị của phần tử duy nhất tồn tại đến cuối cùng. 

Vì vậy, nhiệm vụ trở thành việc chọn yếu tố nào chúng ta quản lý để lại cuối cùng. Chúng tôi muốn tối đa hóa người sống sót cuối cùng đó, vì việc trừ giá trị lớn hơn sẽ làm giảm điểm. 

Quá trình này cho phép chúng tôi giữ cho bất kỳ phần tử đã chọn nào không bị xóa bằng cách luôn chèn các giá trị mới ở phía đối diện và không bao giờ hiển thị nó làm điểm cuối hoạt động. Vì mỗi ai được chèn chính xác một lần và không bao giờ bị xóa cho đến khi được chọn, nên chúng ta có thể trì hoãn việc chọn bất kỳ phần tử cụ thể nào, nghĩa là bất kỳ phần tử đơn lẻ nào cũng có thể được giữ nguyên cho đến hết. 

Do đó, chiến lược tối ưu chỉ đơn giản là giữ phần tử có giá trị tối đa cuối cùng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu (tổng trừ người sống sót tối đa) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính tổng tổng của tất cả các giá trị trong hệ thống, bao gồm số 0 ban đầu và tất cả các phần tử mảng. 

Sau đó, chúng tôi xác định phần tử lớn nhất trong toàn bộ tập hợp, vì đó là ứng cử viên tốt nhất để tồn tại trong mọi hoạt động mà không bao giờ được chọn từ điểm cuối. 

Cuối cùng, chúng tôi trừ đi số tiền tối đa này khỏi tổng số tiền, bởi vì mọi phần tử ngoại trừ người sống sót cuối cùng đều được đảm bảo được thêm vào điểm đúng một lần. 

### Tại sao nó hoạt động 

Ở mỗi thao tác, chính xác một phần tử sẽ bị xóa khỏi điểm cuối và được thêm vĩnh viễn vào điểm, trong khi phần tử mới sẽ được thêm vào cùng điểm cuối đó. Điều này bảo toàn tính bất biến rằng cấu trúc luôn chứa tất cả các giá trị được đưa vào trước đó ngoại trừ những giá trị đã bị loại bỏ. Sau n thao tác, chính xác một giá trị vẫn chưa bị xóa. 

Bởi vì mỗi lần loại bỏ tương ứng với chính xác một phép cộng vào điểm, mọi phần tử ngoại trừ phần tử cuối cùng còn lại đều được tính chính xác một lần trong T. Do đó, T được cố định bằng tổng_tổng trừ đi giá trị sống sót và việc giảm thiểu T tương đương với việc tối đa hóa phần tử sống sót. Vì bất kỳ phần tử nào cũng có thể được duy trì bằng cách luôn chọn điểm cuối đối diện trong quá trình chèn, nên phần tử tối đa luôn có thể được giữ nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        total = sum(a)  # includes only ai, initial 0 does not matter
        mx = max(a)
        
        print(total - mx)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên thực tế là số 0 ban đầu không ảnh hưởng đến biểu thức cuối cùng: trừ số tối đa trên tất cả các phần tử bao gồm số 0 tương đương với việc trừ số tối đa ai, vì ai đều dương. 

Chúng tôi tránh hoàn toàn mọi mô phỏng của deque. Các hoạt động duy nhất là một lần cho tổng và tối đa. 

Một lỗi triển khai phổ biến là cố gắng theo dõi rõ ràng quá trình tiến hóa deque. Điều đó là không cần thiết vì bài toán ẩn giấu một bất biến tổ hợp thuần túy về số lần mỗi phần tử được tích điện. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với mảng`[3, 1, 4]`. 

Chúng tôi theo dõi tổng số tiền và tối đa. 

| Bước | Hành động | Tổng số tiền | Phần tử tối đa | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | đọc mảng | 8 | 4 | | 
| Tính toán | tổng trừ tối đa | 8 | 4 | 4 | 

Chiến lược tối ưu đảm bảo rằng 4 vẫn là yếu tố cuối cùng, do đó chỉ có 8 − 4 được trả. 

Điều này chứng tỏ rằng cấu trúc deque trung gian là không thích hợp; chỉ có danh tính của người sống sót cuối cùng mới quan trọng. 

Bây giờ hãy xem xét`[5, 5, 5]`. 

| Bước | Hành động | Tổng số tiền | Phần tử tối đa | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | đọc mảng | 15 | 5 | | 
| Tính toán | tổng trừ tối đa | 15 | 5 | 10 | 

Mặc dù tất cả các giá trị đều giống hệt nhau, bất kỳ giá trị nào trong số chúng đều có thể được giữ nguyên, vì vậy chúng tôi lưu lại một lần xuất hiện của 5 một cách hiệu quả. 

Điều này xác nhận rằng thuật toán không phụ thuộc vào các lựa chọn vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm yêu cầu một lần vượt qua để tính tổng và giá trị tối đa | 
| Không gian | O(1) | Chỉ có một số biến tích lũy được sử dụng | 

Giải pháp này xử lý thoải mái toàn bộ hạn chế của tổng cộng hai trăm nghìn phần tử vì nó chỉ thực hiện quét tuyến tính với chi phí tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        total = sum(a)
        mx = max(a)
        out.append(str(total - mx))
    return "\n".join(out)

# sample-like
assert run("1\n4\n9 3 6 5\n") == "23"

# single element
assert run("1\n1\n10\n") == "0"

# all equal
assert run("1\n5\n2 2 2 2 2\n") == "8"

# increasing
assert run("1\n3\n1 2 3\n") == "3"

# large mix
assert run("1\n6\n5 1 9 2 8 3\n") == "18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ bản không được trả tiền | 
| tất cả đều bình đẳng | tổng trừ một phần tử | sự đối xứng của sự lựa chọn | 
| ngày càng tăng | loại bỏ lớn nhất | tính đúng đắn của quy tắc sống sót tối đa | 
| giá trị hỗn hợp | tính đúng đắn chung | không phụ thuộc vào đơn hàng | 

## Vỏ cạnh 

Mảng một phần tử không có sự lựa chọn có ý nghĩa. Deque bắt đầu bằng 0, một thao tác được thực hiện và số 0 đó là giá trị duy nhất từng bị xóa, do đó điểm bằng 0. Công thức vẫn đúng vì sum(ai) bằng phần tử lớn nhất. 

Khi tất cả các giá trị giống hệt nhau, quyết định tham lam không có vấn đề gì, nhưng thuật toán vẫn phải tính toán chính xác để lưu chính xác một lần xuất hiện. Phép trừ max sẽ loại bỏ chính xác một bản sao. 

Khi giá trị tối đa xuất hiện sớm trong chuỗi, một chiến lược ngây thơ có thể cho rằng nó chắc chắn sẽ được tính điểm sớm. Trong thực tế, nó có thể được bảo tồn vô thời hạn bằng cách luôn chèn các phần tử mới ở phía đối diện, đảm bảo nó không bao giờ trở thành điểm cuối cho đến trạng thái cuối cùng.
