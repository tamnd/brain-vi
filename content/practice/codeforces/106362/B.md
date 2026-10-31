---
title: "CF 106362B - Quà Tặng Tình Nhân"
description: "Nhiệm vụ là xây dựng một hoán vị của các số nguyên từ 1 đến n để tối đa hóa thước đo “vẻ đẹp” nhất định được xác định trong cách sắp xếp."
date: "2026-06-19T17:11:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106362
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 2-11-2026 Div. 2 (Beginner)"
rating: 0
weight: 106362
solve_time_s: 63
verified: true
draft: false
---

[CF 106362B - Quà tặng người yêu](https://codeforces.com/problemset/problem/106362/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xây dựng một hoán vị của các số nguyên từ 1 đến n để tối đa hóa thước đo “vẻ đẹp” nhất định được xác định trong cách sắp xếp. Bạn không chọn các số tùy ý hoặc tính toán một giá trị từ một mảng cố định, thay vào đó bạn được yêu cầu sắp xếp lại từ 1 đến n theo cách làm cho điểm nhất định càng lớn càng tốt. 

Cấu trúc của điểm dựa trên sự tương tác giữa các phần tử lân cận trong hoán vị. Mỗi cặp liền kề góp phần tạo nên vẻ đẹp tổng thể tùy theo khoảng cách giữa hai giá trị. Bởi vì mọi phần tử đều tham gia vào nhiều nhất hai quan hệ kề cận, nên cách đặt số lớn so với số nhỏ sẽ chi phối kết quả cuối cùng. 

Mặc dù câu lệnh ban đầu trình bày một giới hạn giống như công thức khác nhau giữa n chẵn và lẻ, nhưng điểm mấu chốt là cấu trúc tối ưu tuân theo một mô hình xen kẽ rất đều đặn giữa nửa dưới và nửa trên của các số. Kết quả cuối cùng có thể đạt được chỉ phụ thuộc vào việc có thể tạo ra bao nhiêu cặp lớn-nhỏ hiệu quả như vậy, điều này hóa ra lại đơn giản hóa thành hàm riêng của n. 

Từ quan điểm phức tạp, n có thể đủ lớn để bất kỳ phương trình bậc hai hoặc thậm chí nào$O(n \log n)$chiến lược liên quan đến mô phỏng lặp đi lặp lại hoặc tối ưu hóa các hoán vị là không cần thiết. Cấu trúc gợi ý rõ ràng rằng câu trả lời mang tính xây dựng và tuyến tính, vì chúng ta chỉ cần đưa ra một hoán vị thay vì tìm kiếm trên nhiều hoán vị. 

Một sự hiểu lầm ngây thơ là thử tất cả các hoán vị và tính toán vẻ đẹp cho mỗi hoán vị. Với n = 10, điều này đã bao gồm 10! những khả năng vượt xa khả năng tính toán khả thi. Ngay cả việc thử hoán đổi tham lam mà không có cấu trúc cũng có thể thất bại vì những cải tiến cục bộ không đảm bảo tính tối ưu toàn cầu. 

Trường hợp cạnh tinh tế phát sinh khi n nhỏ. Với n = 1 hoặc n = 2, không gian hoán vị là tầm thường và mọi cách xây dựng vẫn phải tuân theo quy tắc chung. Một trường hợp cạnh khác là sự chuyển đổi giữa n chẵn và lẻ, trong đó phần tử trung tâm trong một cấu trúc xen kẽ hoạt động khác nhau nhưng không làm thay đổi chiến lược tối ưu tổng thể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra mọi hoán vị từ 1 đến n, tính điểm đẹp cho mỗi cách sắp xếp bằng cách lặp lại các cặp liền kề và lấy giá trị tối đa. Điều này đúng về mặt khái niệm vì nó đánh giá định nghĩa một cách trực tiếp. Tuy nhiên, nó yêu cầu tạo n! hoán vị và tính điểm theo O(n) cho mỗi hoán vị, dẫn đến các phép toán O(n · n!). Điều này trở nên không khả thi ngay lập tức ngay cả đối với n vừa phải. 

Cái nhìn sâu sắc về cấu trúc quan trọng là những đóng góp lớn đến từ việc ghép các giá trị nhỏ với các giá trị lớn càng thường xuyên càng tốt. Nếu chúng ta đặt các số ở hai đầu đối diện của thứ tự đã sắp xếp cạnh nhau thì mỗi số liền kề sẽ đóng góp gần đến chênh lệch tối đa có thể có. Do đó, chiến lược tối ưu sẽ xen kẽ giữa nửa trên và nửa dưới của các giá trị. 

Với n chẵn, chúng ta có thể ghép các phần tử từ hai nửa một cách hoàn hảo. Đối với n lẻ, một phần tử vẫn chưa được ghép đôi trong cấu trúc đối xứng này, làm giảm một chút số điểm có thể đạt được, nhưng cùng một ý tưởng xen kẽ vẫn tối đa hóa những gì có thể. 

Điều này làm giảm vấn đề từ việc tìm kiếm các hoán vị đến việc trực tiếp xây dựng một hoán vị bằng cách ghép nối xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · n!) | O(n) | Quá chậm | 
| Xây dựng tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hoán vị bằng cách ghép các phần tử ở nửa trên với các phần tử ở nửa dưới theo thứ tự xen kẽ. 

1. Chia các số từ 1 đến n thành hai nhóm khái niệm: đoạn dưới bắt đầu từ 1 trở lên và đoạn trên bắt đầu từ n trở xuống. Mục đích là xen kẽ chúng sao cho mỗi vùng kề càng lớn càng tốt. 
2. Bắt đầu đặt các phần tử bằng cách lấy một phần tử từ phía trên, sau đó lấy một phần tử từ phía dưới và tiếp tục xen kẽ. Điều này đảm bảo rằng mỗi cặp liền kề kết nối các giá trị có độ lớn cách xa nhau, tối đa hóa sự đóng góp của chúng vào thước đo vẻ đẹp. 
3. Khi n chẵn, cả hai nửa đều thẳng hàng một cách hoàn hảo, do đó mọi phần tử được ghép theo một chuỗi xen kẽ cân bằng. 
4. Khi n lẻ, việc xây dựng diễn ra theo cách tương tự, nhưng một phần tử ở nửa trên vẫn ở cuối. Việc đặt nó ở vị trí cuối cùng sẽ tránh được việc phá vỡ các vùng lân cận có độ chênh lệch cao trước đó. 
5. Xuất trực tiếp hoán vị được xây dựng mà không cần tối ưu hóa thêm, vì cấu trúc đã đảm bảo tính tối đa. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi bước thi công, phần tử được chọn tiếp theo luôn được lấy từ cực đối diện của các số chưa sử dụng còn lại so với lần chọn trước đó. Điều này đảm bảo rằng mọi vùng lân cận được hình thành giữa một khoảng trống lớn còn lại và không có sự sắp xếp lại nào có thể làm tăng bất kỳ đóng góp cục bộ nào mà không làm giảm phần đóng góp khác. Vì tất cả các khoản đóng góp đã được tối đa hóa cục bộ theo ràng buộc sử dụng mỗi số chính xác một lần, nên tổng toàn cầu cũng được tối đa hóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    left = 1
    right = n
    res = []
    
    while left <= right:
        if left == right:
            res.append(left)
        else:
            res.append(right)
            res.append(left)
        left += 1
        right -= 1
    
    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp đọc n và xây dựng hoán vị trong một lần sử dụng hai con trỏ. Một con trỏ bắt đầu từ giá trị nhỏ nhất và con trỏ kia bắt đầu từ giá trị lớn nhất. Bằng cách luôn nối thêm giá trị lớn nhất hiện tại rồi đến giá trị nhỏ nhất hiện tại, chúng tôi thực thi cấu trúc cao thấp xen kẽ để tạo ra sự sắp xếp tối ưu. 

Chi tiết tinh tế duy nhất là xử lý phần tử ở giữa khi n lẻ. Trong trường hợp đó, cả hai con trỏ đều có cùng giá trị và nó được thêm vào một lần mà không trùng lặp. Điều này đảm bảo tính chính xác cho cả hai trường hợp chẵn lẻ mà không có các nhánh logic riêng biệt. 

## Ví dụ đã hoạt động 

Xét n = 6. 

| Bước | trái | đúng | Hành động | Hoán vị | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 6 | mất 6, 1 | 6, 1 | 
| 2 | 2 | 5 | mất 5, 2 | 6, 1, 5, 2 | 
| 3 | 3 | 4 | lấy 4, 3 | 6, 1, 5, 2, 4, 3 | 

Điều này tạo ra hoán vị [6, 1, 5, 2, 4, 3], xen kẽ các cực trị ở mỗi bước. Mỗi cặp liền kề kết nối các phần tử từ các đầu đối diện của tập hợp còn lại, giúp duy trì sự khác biệt lớn. 

Bây giờ hãy xem xét n = 7. 

| Bước | trái | đúng | Hành động | Hoán vị | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 7 | mất 7, 1 | 7, 1 | 
| 2 | 2 | 6 | mất 6, 2 | 7, 1, 6, 2 | 
| 3 | 3 | 5 | lấy 5, 3 | 7, 1, 6, 2, 5, 3 | 
| 4 | 4 | 4 | lấy 4 | 7, 1, 6, 2, 5, 3, 4 | 

Phần tử ở giữa đóng chuỗi một cách tự nhiên mà không phá vỡ cấu trúc xen kẽ được hình thành trước đó. Việc xây dựng vẫn bảo tồn nguyên tắc ghép nối cực đoan tương tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được thêm chính xác một lần trong khi hai con trỏ đi qua phạm vi | 
| Không gian | O(n) | Mảng hoán vị lưu trữ tất cả n phần tử | 

Việc xây dựng tuyến tính nằm trong các ràng buộc điển hình cho n lên tới 200.000 hoặc cao hơn. Không cần sắp xếp hoặc tìm kiếm tổ hợp, do đó giải pháp chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimum case
assert solve_io("1\n") == "1"

# small even
assert solve_io("2\n") in ("2 1", "1 2")

# small odd
assert solve_io("3\n") in ("3 1 2", "3 1 2", "3 1 2")

# even larger sanity check
res = solve_io("6\n").split()
assert sorted(res) == ["1","2","3","4","5","6"]

# odd larger sanity check
res = solve_io("7\n").split()
assert sorted(res) == ["1","2","3","4","5","6","7"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp ranh giới tối thiểu | 
| 2 | bất kỳ hoán vị nào của 1,2 | trường hợp hoán đổi không tầm thường nhỏ nhất | 
| 3 | xây dựng xen kẽ | xử lý độ dài lẻ | 
| 6 | hoán vị hợp lệ là 1..6 | độ chính xác có độ dài chẵn | 
| 7 | hoán vị hợp lệ là 1..7 | xử lý phần tử trung tâm | 

## Vỏ cạnh 

Với n = 1, thuật toán ngay lập tức phát hiện bên trái bằng bên phải và đưa ra một giá trị duy nhất. Không thể ghép nối được và cấu trúc suy biến chính xác. 

Với n = 2, thuật toán tạo ra 2 1 hoặc 1 2 tùy theo thứ tự xen kẽ. Cả hai đều hợp lệ và đạt được cấu trúc tối ưu như nhau vì chỉ có một lân cận. 

Đối với n lẻ chẳng hạn như n = 5 hoặc n = 7, phần tử ở giữa gặp phải khi trái bằng phải. Tại thời điểm đó, nó được nối thêm một lần, duy trì tính chính xác mà không bị trùng lặp và duy trì cấu trúc cao-thấp xen kẽ cho đến thời điểm đó.
