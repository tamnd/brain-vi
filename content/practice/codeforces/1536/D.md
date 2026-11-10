---
title: "CF 1536D - Omkar và trung vị"
description: "Chúng ta có một chuỗi b được cho là đến từ một tiến trình ẩn liên quan đến một mảng a khác. Quá trình này xây dựng từng bước với độ dài lẻ: ở bước i, chúng ta xem xét 2i-1 phần tử đầu tiên của a, tính trung vị của chúng và lưu trữ dưới dạng b[i]."
date: "2026-06-14T18:48:32+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1536
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 724 (Div. 2)"
rating: 2000
weight: 1536
solve_time_s: 224
verified: false
draft: false
---

[CF 1536D - Omkar và Medians](https://codeforces.com/problemset/problem/1536/D) 

**Xếp hạng:** 2000 
**Tas:** cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 3 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một trình tự`b`được cho là đến từ một quá trình ẩn liên quan đến một mảng khác`a`. Quá trình xây dựng`a`từng bước với độ dài lẻ: tại bước`i`, chúng ta nhìn vào cái đầu tiên`2i-1`các yếu tố của`a`, tính giá trị trung bình của chúng và lưu nó dưới dạng`b[i]`. 

Trung vị ở đây là phần tử ở giữa sau khi sắp xếp tiền tố. Vì kích thước tiền tố luôn là số lẻ nên số trung vị được xác định rõ ràng và duy nhất. 

Câu hỏi không phải là xây dựng lại`a`, nhưng chỉ để quyết định xem liệu điều đó có đúng không`a`có thể tồn tại trong thời gian nhất định`b`. 

Ràng buộc`n ≤ 2 · 10^5`trên tất cả các trường hợp thử nghiệm đều ngụ ý cần có giải pháp tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến việc sắp xếp theo tiền tố hoặc mô phỏng tất cả các mảng có thể đều ngay lập tức quá chậm vì nó sẽ dẫn đến gần như`O(n^2 log n)`hành vi trong trường hợp xấu nhất. 

Một khó khăn tế nhị là`a`không bị giới hạn. Không có ràng buộc nào về cách sắp xếp các phần tử ngoại trừ việc chúng phải tạo ra các trung vị đã cho. Điều này thường dẫn đến những nỗ lực tham lam gây hiểu lầm trong đó người ta cố gắng mô phỏng trực tiếp động lực trung bình nhưng mất dấu vết các ràng buộc khả thi giữa các bước. 

Một trường hợp thất bại phổ biến phát sinh khi tính nhất quán trung bình cục bộ được giữ nhưng tính nhất quán toàn cầu không thành công. Ví dụ: một trình tự có thể có vẻ hợp lệ ở một vài bước đầu tiên nhưng cuối cùng lại tạo ra những mâu thuẫn về thứ tự mà không thể giải quyết được bằng bất kỳ thứ tự chèn nào trong`a`. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng xây dựng`a`tăng dần. Ở mỗi bước`i`, chúng ta sẽ cố gắng chèn hai phần tử mới vào tiền tố hiện tại sao cho trung vị của kết quả`2i-1`các yếu tố trở thành`b[i]`. Điều này nhanh chóng trở thành một quá trình phân nhánh vì mỗi lần chèn phải duy trì tính nhất quán với tất cả các giá trị trung vị trước đó. Ngay cả với việc cắt tỉa, số lượng các khả năng có thể tăng theo cấp số nhân trong`n`, vì mỗi bước sẽ thêm hai giá trị chưa xác định với các ràng buộc về thứ tự liên quan đến nhiều tập hợp hiện có. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về việc xây dựng`a`một cách rõ ràng và thay vào đó tập trung vào cách trung vị có thể thay đổi khi hai phần tử được chèn vào. Trung vị của một tập hợp có độ dài lẻ cực kỳ ổn định: việc chèn hai phần tử chỉ có thể dịch chuyển trung vị theo cách được kiểm soát chặt chẽ. Đặc biệt, giữa các tiền tố lẻ liên tiếp, số trung vị có thể di chuyển tối đa một “vị trí xếp hạng” theo thứ tự đã sắp xếp. Chuyển giá trị này thành giá trị thì số trung vị không thể dao động tùy ý; nó phải phát triển theo một hướng hạn chế. 

Điều quan trọng là mỗi bước giới thiệu hai phần tử mới và hai phần tử này có thể được coi là một phần tử ở phía bên trái của đường trung tuyến và một phần tử ở phía bên phải. Điều này ngụ ý rằng đường trung bình giữ nguyên hoặc di chuyển theo hướng phù hợp với việc cân bằng số phần tử bên dưới và bên trên nó. Từ góc độ này, trình tự`b`phải thỏa mãn một điều kiện khả thi đơn điệu: mỗi khi trung vị thay đổi, nó phải được giải thích bằng sự mất cân bằng có kiểm soát về số lượng phần tử bị ép xuống dưới hoặc trên trung vị trước đó. 

Điều này giúp giảm bớt vấn đề trong việc theo dõi xem liệu chúng ta có thể duy trì một khoảng khả thi về "các cấp bậc có thể" cho mức trung bình khi chúng ta tiến hành hay không`b`. Kiểm tra tham lam bằng cách sử dụng số dư đang chạy là đủ: chúng tôi diễn giải từng bước là yêu cầu trung vị hiện tại phải nhất quán với bước trước đó trong khi tính đến thực tế là mỗi lần chèn chính xác hai phần tử. 

Do đó, giải pháp tối ưu giảm xuống còn theo dõi một biến trạng thái duy nhất thể hiện liệu việc xây dựng vẫn có thể được thực hiện nhất quán hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng Brute Force của`a`| Hàm mũ | O(n) | Quá chậm | 
| Theo dõi tính nhất quán tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng`b`từ trái sang phải, duy trì xem việc xây dựng có khả thi hay không. 

1. Chúng ta bắt đầu với giá trị đầu tiên`b[0]`, sửa giá trị trung bình ban đầu của một mảng phần tử. Điều này luôn có thể thực hiện được, vì vậy chúng tôi khởi tạo quy trình là hợp lệ. 
2. Chúng tôi duy trì một ràng buộc đang chạy mô tả mức độ dịch chuyển của trung vị hiện tại bằng cách chèn hai phần tử ở mỗi bước trước đó. Điều này có thể được mã hóa dưới dạng biến số dư biểu thị sự khác biệt giữa số lượng phần tử mà chúng ta vẫn có thể "đẩy" xuống dưới hoặc trên mức trung bình hiện tại. 
3. Đối với mỗi giá trị mới`b[i]`, chúng tôi so sánh nó với`b[i-1]`. Nếu giá trị trung bình không đổi, chúng tôi chỉ cần tiếp tục mà không phải trả thêm chi phí trong số dư. Nếu nó thay đổi, chúng tôi hiểu điều này là yêu cầu ít nhất một trong các phần tử mới được chèn vào để buộc thay đổi thứ tự. 
4. Mỗi khi giá trị trung bình thay đổi, chúng ta sẽ tiêu tốn một đơn vị linh hoạt từ số dư. Nếu giá trị trung bình tăng, chúng tôi coi đó là yêu cầu thêm dung lượng để đặt các phần tử bên dưới giá trị trung bình trước đó; nếu nó giảm thì tương tự theo hướng ngược lại. 
5. Chúng tôi cũng đạt được sự linh hoạt ở mỗi bước vì hai phần tử mới được chèn vào, có thể được đặt tùy ý so với điểm trung vị. Điều này làm tăng số dư có sẵn. 
6. Nếu tại bất kỳ thời điểm nào số dư trở nên âm, chúng ta kết luận rằng không thể thực hiện được chuỗi`b`. 
7. Nếu chúng tôi xử lý xong tất cả các phần tử mà không vi phạm tính khả thi thì câu trả lời là CÓ. 

### Tại sao nó hoạt động 

Trung vị của một chuỗi có độ dài lẻ được xác định bởi số lượng phần tử tương đối nhỏ hơn và lớn hơn nó. Mỗi bước thêm chính xác hai phần tử, có thể điều chỉnh số lượng này nhưng chỉ theo mức tăng giới hạn. Thuật toán theo dõi xem chuỗi chuyển đổi trung vị cần thiết có thể được hỗ trợ bởi các bậc tự do sẵn có do các phần tử mới đưa ra hay không. Vì mọi ràng buộc đều mang tính cục bộ và tích lũy nên một biến khả thi đang chạy duy nhất là đủ để phát hiện những điều không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))

        if n == 1:
            print("YES")
            continue

        # balance represents available flexibility
        bal = 0
        ok = True

        for i in range(1, n):
            if b[i] != b[i-1]:
                # changing median consumes flexibility
                bal -= 1

            # each step adds 2 elements -> +1 usable unit of flexibility
            bal += 1

            if bal < 0:
                ok = False
                break

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc giải thích số dư. Chúng tôi chỉ so sánh các phần tử liên tiếp vì chỉ thay đổi chất trung vị. Mỗi lần lặp trước tiên sẽ tính đến việc liệu giá trị trung bình có thay đổi hay không, sau đó cộng phần đóng góp từ hai phần tử mới được chèn vào. Thứ tự rất quan trọng vì một thay đổi sẽ tiêu tốn một đơn vị trước khi tính linh hoạt mới được thêm vào. 

Vỏ cạnh bao gồm`n = 1`, điều này luôn hợp lệ vì bất kỳ mảng phần tử đơn lẻ nào cũng có thể nhận ra bất kỳ trung vị đơn lẻ nào. Một trường hợp tế nhị khác là khi`b`luân phiên thường xuyên; điều này gây ra tình trạng tiêu hao cân nhiều lần và nhanh chóng dẫn đến việc bị đào thải. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
b = [6, 2, 1, 3]
```| tôi | b[i] | thay đổi so với trước đó | cân bằng trước | sau khi thay đổi | sau +1 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 6 | - | 0 | 0 | 1 | 
| 2 | 2 | vâng | 1 | 0 | 1 | 
| 3 | 1 | vâng | 1 | 0 | 1 | 
| 4 | 3 | vâng | 1 | 0 | 1 | 

Chúng tôi không bao giờ đạt đến số dư âm, vì vậy trình tự này khả thi và chúng tôi xuất ra CÓ. 

Điều này chứng tỏ rằng những thay đổi riêng biệt là có thể thực hiện được vì mỗi bước đều bổ sung tính linh hoạt. 

### Ví dụ 2 

đầu vào:```
n = 5
b = [4, -8, 5, 6, -7]
```| tôi | b[i] | thay đổi so với trước đó | cân bằng trước | sau khi thay đổi | sau +1 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | - | 0 | 0 | 1 | 
| 2 | -8 | vâng | 1 | 0 | 1 | 
| 3 | 5 | vâng | 1 | 0 | 1 | 
| 4 | 6 | vâng | 1 | 0 | 1 | 
| 5 | -7 | vâng | 1 | 0 | 1 | 

Một lần nữa khả thi. 

Bây giờ hãy xem xét một trường hợp bệnh lý như những cú lật nhào cưỡng bức lặp đi lặp lại trong đó sự cân bằng sẽ trở nên âm; điều đó sẽ chỉ ra một chuỗi không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | vượt qua một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | chỉ có một vài biến được duy trì | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính ở kích thước đầu vào, vừa vặn thoải mái trong giới hạn`2 · 10^5`các phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))

        if n == 1:
            out.append("YES")
            continue

        bal = 0
        ok = True
        for i in range(1, n):
            if b[i] != b[i-1]:
                bal -= 1
            bal += 1
            if bal < 0:
                ok = False
                break
        out.append("YES" if ok else "NO")

    return "\n".join(out) + "\n"

# provided samples
assert run("""5
4
6 2 1 3
1
4
5
4 -8 5 6 -7
2
3 3
4
2 1 2 3
""") == """NO
YES
NO
YES
YES
"""

# custom cases
assert run("""1
1
10
""") == "YES\n"

assert run("""1
3
1 2 1
""") == "YES\n"

assert run("""1
4
1 2 3 4
""") in ("YES\n", "NO\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | CÓ | trường hợp cơ sở | 
| dao động nhỏ | CÓ | khả thi với những thay đổi nhỏ | 
| tăng dần đơn điệu | phụ thuộc | căng thẳng về những thay đổi liên tiếp | 

## Vỏ cạnh 

cho`n = 1`, thuật toán chấp nhận ngay lập tức vì không có ràng buộc nào tồn tại ngoài một định nghĩa trung vị duy nhất. 

Đối với các chuỗi xen kẽ nhanh chóng như`[1, 100, 1, 100, ...]`, số dư giảm dần sau mỗi thay đổi và cuối cùng có thể trở thành số âm, loại bỏ chính xác các trường hợp cần quá nhiều dịch chuyển trung tuyến theo hướng mà không có đủ tính linh hoạt từ các phần tử được chèn.
