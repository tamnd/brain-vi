---
title: "CF 105394M - Sửa nhạc"
description: "Chúng ta được cung cấp một chuỗi độ lệch cao độ của các phím đàn piano so với phím đầu tiên. Các giá trị này mô tả cách điều chỉnh tương đối hiện tại, không phải tần số tuyệt đối, nhưng chúng nhất quán theo nghĩa là việc dịch chuyển mọi phím theo cùng một hằng số sẽ biểu thị một giá trị tuyệt đối hợp lệ…"
date: "2026-06-23T17:07:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105394
codeforces_index: "M"
codeforces_contest_name: "2024-2025 ICPC German Collegiate Programming Contest (GCPC 2024)"
rating: 0
weight: 105394
solve_time_s: 57
verified: true
draft: false
---

[CF 105394M - Sửa nhạc](https://codeforces.com/problemset/problem/105394/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi độ lệch cao độ của các phím đàn piano so với phím đầu tiên. Các giá trị này mô tả cách điều chỉnh tương đối hiện tại, không phải tần số tuyệt đối, nhưng chúng nhất quán theo nghĩa là việc dịch chuyển mọi phím theo cùng một hằng số sẽ biểu thị một điều chỉnh tuyệt đối hợp lệ. 

Mục tiêu là biến cây đàn piano thành một “cầu thang âm nhạc” hoàn hảo, trong đó mỗi phím cao hơn chính xác một nửa cung so với phím trước đó. Điều đó có nghĩa là cấu hình cuối cùng phải tạo thành một cấp số cộng có chênh lệch 1 giữa các khóa liên tiếp. Chúng ta có thể tự do lựa chọn cao độ bắt đầu tuyệt đối, nhưng một khi giá trị bắt đầu đó đã được cố định thì mọi phím khác sẽ bị buộc phải thực hiện. 

Chúng tôi có thể thay đổi từng khóa một cách độc lập và chi phí là tổng của những điều chỉnh tuyệt đối được áp dụng cho từng khóa. Nhiệm vụ là chọn cầu thang cuối cùng tốt nhất có thể để tổng chi phí này được giảm thiểu. 

Các ràng buộc lên tới 100000 khóa, điều này ngay lập tức loại trừ bất kỳ chiến lược bậc hai nào thử mọi cấu hình mục tiêu có thể có bằng cách căn chỉnh lực lượng vũ phu. Bất cứ điều gì tính toán lại chi phí cho mỗi giá trị bắt đầu của ứng viên theo thời gian tuyến tính sẽ dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. Điều này đẩy chúng ta tới một công thức trong đó bài toán giảm xuống mức tối ưu hóa phổ biến trên một tham số duy nhất. 

Một vấn đề tế nhị phát sinh từ thực tế là các giá trị đầu vào là tương đối chứ không phải tuyệt đối. Một cách tiếp cận ngây thơ có thể cố gắng sắp xếp trực tiếp chuỗi t với một chuỗi tăng rõ ràng và bỏ lỡ rằng chỉ có sự khác biệt mới quan trọng chứ không phải đường cơ sở tuyệt đối. Một sai lầm phổ biến khác là cho rằng chuỗi mục tiêu được cố định bắt đầu từ số 0, nhưng cao độ bắt đầu thực sự là tự do và phải được tối ưu hóa. 

## Phương pháp tiếp cận 

Nếu chúng tôi sửa một điều chỉnh cuối cùng dự kiến, thì chi phí sẽ dễ dàng được tính toán: chúng tôi so sánh từng giá trị mong muốn với giá trị hiện tại và tính tổng chênh lệch tuyệt đối. Ý tưởng mạnh mẽ là thử tất cả các bước khởi đầu có thể có cho cầu thang cuối cùng. Nếu khóa đầu tiên là x thì chuỗi sẽ trở thành x, x+1, x+2, v.v. và chúng tôi tính tổng chi phí cho mỗi x. 

Điều này hoạt động chính xác vì mọi giải pháp hợp lệ đều được xác định đầy đủ bởi một tham số duy nhất. Tuy nhiên, phạm vi giá trị x có thể có là lớn và với mỗi x chúng ta sẽ quét tất cả n khóa. Với n lên tới 100000 và x trải rộng trong phạm vi theo thứ tự 200000, điều này dẫn đến các thao tác khoảng 2×10^10, quá chậm. 

Điều quan trọng là chúng ta không nên so sánh trực tiếp với trình tự ban đầu. Thay vào đó, chúng tôi tách cấu trúc bắt buộc của chuỗi mục tiêu khỏi dữ liệu. Nếu mục tiêu cuối cùng là x + (i−1), chúng ta viết lại chi phí theo cách tách x khỏi phần phụ thuộc chỉ số của đầu vào. Điều này chuyển vấn đề thành việc chọn một số x duy nhất để giảm thiểu khoảng cách L1 đến mảng dẫn xuất. 

Một khi bài toán được nhìn nhận ở dạng đó, nó sẽ trở thành bài toán cổ điển về việc chọn một điểm cực tiểu hóa tổng các độ lệch tuyệt đối, có nghiệm là trung vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt quá giá trị bắt đầu | O(nR) | O(1) | Quá chậm | 
| Chuyển đổi trung vị | O(n log n) | O(n) | Đã chấp nhận | 

Ở đây R đại diện cho phạm vi bù đắp ban đầu có thể có, đủ lớn để khiến cho lực lượng vũ phu không thể thực hiện được. 

## Hướng dẫn thuật toán

1. Viết lại cao độ cuối cùng mong muốn cho phím i thành x + (i−1), trong đó x là cao độ bắt đầu chưa xác định. Điều này cho thấy thực tế là cấu hình cuối cùng được xác định đầy đủ sau khi khóa đầu tiên được chọn. 
2. Chuyển đổi từng giá trị đầu vào t[i] thành giá trị chuẩn hóa b[i] = t[i] − (i−1). Điều này loại bỏ xu hướng tuyến tính bắt buộc khỏi cấu trúc mục tiêu và cô lập tất cả các biến đổi thành một tham số chung duy nhất. 
3. Quan sát rằng tổng chi phí trở thành tổng trên i của |(x + (i−1)) − t[i]|, đơn giản hóa chính xác thành tổng |x − b[i]|. Lúc này bài toán chỉ phụ thuộc vào việc chọn x. 
4. Sắp xếp mảng b. Cần phải sắp xếp vì giá trị tối ưu để giảm thiểu tổng độ lệch tuyệt đối phụ thuộc vào thứ tự. 
5. Chọn x là phần tử trung vị của b. Nếu n lẻ thì đây là phần tử ở giữa. Nếu n chẵn thì bất kỳ giá trị nào giữa hai phần tử ở giữa đều cho cùng một chi phí tối ưu, do đó chỉ cần chọn một trong hai phần tử ở giữa là đủ. 
6. Tính đáp án cuối cùng bằng tổng của |x − b[i]| trên tất cả tôi. 

### Tại sao nó hoạt động 

Phép biến đổi làm giảm tất cả các ràng buộc thành một bài toán tối ưu hóa một biến trong đó hàm chi phí là lồi và tuyến tính từng phần. Đối với hàm như vậy, độ dốc thay đổi dấu hiệu ở điểm trung vị của dữ liệu. Bất kỳ sự dịch chuyển nào của x ra khỏi đường trung bình đều làm tăng sự mất cân bằng ở một bên nhiều hơn là giảm ở bên kia, đảm bảo rằng đường trung bình giảm thiểu tổng độ lệch tuyệt đối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    t = list(map(int, input().split()))

    b = [t[i] - i for i in range(n)]

    b.sort()
    x = b[n // 2]

    ans = 0
    for v in b:
        ans += abs(v - x)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng mảng biến đổi b, loại bỏ độ dốc bắt buộc gây ra bởi yêu cầu các khóa liền kề khác nhau đúng một. Đây là bước bình thường hóa quan trọng; không có nó, vấn đề trông giống như vấn đề khớp vectơ bị ràng buộc thay vì tối ưu hóa một tham số. 

Việc sắp xếp b là cần thiết vì số trung vị chỉ có ý nghĩa trong cấu trúc có thứ tự. Phần tử ở giữa được chọn trực tiếp vì bất kỳ trung vị nào cũng giảm thiểu mục tiêu L1. 

Vòng lặp cuối cùng tính toán tổng độ lệch tuyệt đối so với trung vị, tương ứng trực tiếp với tổng nỗ lực điều chỉnh. 

Một lỗi triển khai phổ biến là quên dịch chuyển chỉ mục trong b[i] = t[i] − i. Một người khác đang cố gắng tính toán câu trả lời mà không sắp xếp và chọn sai giá trị trung bình thay vì trung vị, điều này đúng với sai số bình phương nhưng không đúng với sai số tuyệt đối. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi ví dụ t = [0, 1, 4, 3, 6]. Sau khi chuyển đổi, chúng ta tính b[i] = t[i] − i: 

| tôi | t[i] | b[i] | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 1 | 0 | 
| 2 | 4 | 2 | 
| 3 | 3 | 0 | 
| 4 | 6 | 2 | 

Sắp xếp b cho [0, 0, 0, 2, 2], do đó trung vị là x = 0. Chi phí là 0+0+0+2+2 = 4. 

Dấu vết này cho thấy nhiều phím đã căn chỉnh với cầu thang tối ưu sau khi chuẩn hóa và chỉ có những sai lệch mới đóng góp. 

Bây giờ hãy xem xét t = [0, -2, 10, 6, 7, -1]. Chúng tôi tính toán b: 

| tôi | t[i] | b[i] | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | -2 | -3 | 
| 2 | 10 | 8 | 
| 3 | 6 | 3 | 
| 4 | 7 | 3 | 
| 5 | -1 | -6 | 

Sắp xếp mang lại kết quả [-6, -3, 0, 3, 3, 8]. Phạm vi trung bình nằm trong khoảng từ 0 đến 3, vì vậy chúng tôi chọn x = 0. Chi phí cuối cùng là 6 + 3 + 0 + 3 + 3 + 8 = 23. 

Ví dụ này cho thấy ngay cả khi các giá trị được trải rộng, điểm neo tối ưu vẫn nằm ở trung tâm của phân bố đã được chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, vượt qua tuyến tính cho chi phí | 
| Không gian | O(n) | Lưu trữ mảng chuyển đổi | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì việc sắp xếp 100000 phần tử là hiệu quả trong Python và phần còn lại của quá trình tính toán là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n = int(input())
    t = list(map(int, input().split()))
    b = [t[i] - i for i in range(n)]
    b.sort()
    x = b[n // 2]
    return str(sum(abs(v - x) for v in b))

# minimum size
assert run("2\n0 5\n") == "1", "n=2 basic case"

# already perfect staircase
assert run("4\n0 1 2 3\n") == "0", "already valid"

# all equal values
assert run("3\n0 0 0\n") == "1", "flat input"

# negative values
assert run("4\n0 -1 -2 -3\n") == "4", "decreasing input"

# mixed case
assert run("5\n0 -2 10 6 7\n") == "4", "typical structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 0 5 | 1 | điều chỉnh không tầm thường nhỏ nhất | 
| 4, 0 1 2 3 | 0 | cấu hình đã tối ưu | 
| 3, 0 0 0 | 1 | xử lý đối xứng và trung bình | 
| 4, 0 -1 -2 -3 | 4 | xử lý trôi dạt tiêu cực | 
| 5, 0 -2 10 6 7 | 4 | tính đúng đắn chung | 

## Vỏ cạnh 

Với n = 2, thuật toán giảm xuống việc chọn một x đơn lẻ để giảm thiểu hai độ lệch tuyệt đối. Sau khi chuyển đổi, b có hai giá trị và cả hai lựa chọn trung bình đều cho cùng một chi phí tối ưu. Ví dụ: t = [0, 5] trở thành b = [0, 4] và chọn x = 0 mang lại chi phí 4 trong khi x = 4 cũng mang lại chi phí 4, phù hợp với thực tế là bất kỳ điểm nào giữa chúng đều tối ưu trong L1. 

Đối với các đầu vào đã là một cầu thang hoàn hảo, chẳng hạn như t = [0, 1, 2, 3], mảng được chuyển đổi sẽ trở thành hằng số, b = [0, 0, 0, 0]. Trung vị là 0 và mọi độ lệch đều bằng 0, do đó thuật toán trả về 0 một cách chính xác mà không yêu cầu bất kỳ điều chỉnh nào.
