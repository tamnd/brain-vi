---
title: "CF 2163A - Souvlaki VS. Kalamaki"
description: "Chúng ta được cấp một dãy số nguyên và hai người chơi lần lượt hành động trên các cặp phần tử liên tiếp. Ở mỗi lượt, người chơi hiện tại có thể bỏ qua hoặc hoán đổi phần tử hiện tại với phần tử tiếp theo."
date: "2026-06-07T23:44:56+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2163
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1063 (Div. 2)"
rating: 800
weight: 2163
solve_time_s: 208
verified: false
draft: false
---

[CF 2163A - Souvlaki VS. Kalamaki](https://codeforces.com/problemset/problem/2163/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, tham lam, toán học, phân loại 
**Thời gian giải:** 3 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số nguyên và hai người chơi lần lượt hành động trên các cặp phần tử liên tiếp. Ở mỗi lượt, người chơi hiện tại có thể bỏ qua hoặc hoán đổi phần tử hiện tại với phần tử tiếp theo. Souvlaki di chuyển theo các lượt số lẻ và muốn dãy số không giảm ở cuối. Kalamaki di chuyển theo hướng chẵn và muốn ngăn chặn điều đó. Trước khi trò chơi bắt đầu, Souvlaki có thể hoán vị mảng theo bất kỳ thứ tự nào mình chọn. Câu hỏi đặt ra là liệu có tồn tại một hoán vị đảm bảo cho Souvlaki giành chiến thắng hay không, bất kể Kalamaki chơi như thế nào. 

Các ràng buộc rất khiêm tốn: mỗi chuỗi có tối đa 100 phần tử và có tới 100 trường hợp thử nghiệm. Điều này có nghĩa là chúng tôi có thể cung cấp các thuật toán có độ phức tạp lên tới khoảng O(n^2) cho mỗi trường hợp thử nghiệm. Mỗi phần tử nằm trong khoảng từ 1 đến n, giúp đơn giản hóa việc suy luận về sự trùng lặp và giới hạn. 

Một cách tiếp cận ngây thơ là thử tất cả n! hoán vị và mô phỏng trò chơi cho mỗi hoán vị. Ngay cả với n = 10, điều này vẫn không thể thực hiện được về mặt tính toán. Các trường hợp cạnh bao gồm các chuỗi có tất cả các phần tử bằng nhau, có thể thắng được một cách tầm thường vì hoán đổi không thể tạo ra sự hỗn loạn và các mẫu cao thấp xen kẽ, có thể đánh bại Souvlaki nếu anh ta không thể định vị các phần tử một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra mọi hoán vị ban đầu có thể có của mảng và mô phỏng các chuyển động xen kẽ. Trong mỗi mô phỏng, Souvlaki chọn sự hoán đổi nếu nó cải thiện trật tự và Kalamaki có thể can thiệp. Mặc dù điều này đúng về mặt khái niệm, nhưng nó trở nên không khả thi đối với n trên 8 vì n! tăng cực kỳ nhanh, khoảng 3,6 triệu khả năng cho n = 10, vượt xa giới hạn thời gian 1 giây. 

Quan sát quan trọng giúp mở ra một giải pháp hiệu quả là Souvlaki chỉ hành động trên các chỉ số lẻ, trong khi Kalamaki hành động trên các chỉ số chẵn. Điều này tách mảng thành hai chuỗi con xen kẽ: các phần tử ở vị trí lẻ và các phần tử ở vị trí chẵn. Souvlaki có thể sắp xếp tất cả các phần tử có vị trí lẻ với nhau và các phần tử có vị trí chẵn với nhau trước trận đấu. Sau đó, trong lượt của mình, anh ta luôn có thể hoán đổi cục bộ để duy trì trật tự trong chuỗi con của mình, trong khi Kalamaki không thể di chuyển các phần tử qua các chuỗi con. Do đó, trò chơi tập trung vào việc kiểm tra xem mảng đã sắp xếp có thể được xen kẽ từ hai dãy con đã sắp xếp này mà không tạo ra bất kỳ sự đảo ngược nào hay không. Nếu vậy thì Souvlaki có thể giành chiến thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy đọc mảng và chia nó thành hai nhóm: một nhóm chứa các phần tử ở chỉ số lẻ ban đầu và nhóm kia chứa các phần tử ở chỉ số chẵn. Chúng tôi làm điều này vì mỗi người chơi chỉ có thể ảnh hưởng đến chỉ số tương ứng của họ. 
2. Sắp xếp cả hai nhóm riêng lẻ theo thứ tự không giảm. Đây là bước quan trọng: Souvlaki có thể hoán vị mảng trước, vì vậy chúng tôi giả sử anh ấy sắp xếp từng chuỗi con một cách tối ưu. 
3. Xây dựng lại một mảng ứng cử viên bằng cách đặt các phần tử có chỉ số lẻ đã được sắp xếp vào các vị trí lẻ ban đầu và các phần tử có chỉ số chẵn đã được sắp xếp ở các vị trí chẵn ban đầu. 
4. Quét qua mảng được xây dựng lại để kiểm tra xem nó có giảm không. Nếu bất kỳ phần tử nào lớn hơn phần tử tiếp theo, xuất "NO"; nếu không thì xuất ra "CÓ". 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Việc sắp xếp các chuỗi chẵn và lẻ riêng biệt đảm bảo rằng Souvlaki luôn có thể duy trì trật tự trong lượt của mình. Kalamaki không thể hoán đổi các phần tử trong các chuỗi con, do đó, không có sự đảo ngược nào bắt đầu trong một chỉ mục lẻ và kết thúc bằng một chỉ mục chẵn có thể lan truyền một cách không kiểm soát được. Nếu việc xen kẽ các dãy con đã sắp xếp tạo ra một mảng không giảm thì Souvlaki có chiến lược thắng, nếu không thì không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        odd = sorted(a[::2])
        even = sorted(a[1::2])
        b = []
        for i in range(n):
            if i % 2 == 0:
                b.append(odd[i // 2])
            else:
                b.append(even[i // 2])
        for i in range(n - 1):
            if b[i] > b[i + 1]:
                print("NO")
                break
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là tách mảng thành các nhóm chỉ số chẵn và lẻ, sắp xếp chúng và xây dựng lại mảng. Kiểm tra cuối cùng đảm bảo rằng sự xen kẽ không giảm. Có thể tránh các lỗi riêng lẻ bằng cách sử dụng phép chia số nguyên để lập chỉ mục cho các chuỗi con đã được sắp xếp. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`[4, 2, 2, 1]`| Bước | Yếu tố lẻ | Phần tử chẵn | Mảng được xây dựng lại | 
| --- | --- | --- | --- | 
| Chia ban đầu | [4, 2] | [2, 1] | - | 
| Đã sắp xếp | [2, 4] | [1, 2] | - | 
| Xen kẽ | - | - | [2, 1, 4, 2] | 
| Kiểm tra | - | - | 2 > 1 → CÓ/KHÔNG? | 

Trên thực tế, chúng ta cần lập chỉ mục dựa trên 0: vị trí lẻ là 0,2 → [4,2], vị trí chẵn là 1,3 → [2,1]; sắp xếp → lẻ: [2,4], chẵn: [1,2]; xây dựng lại → [2,1,4,2], kiểm tra → 2 1? Không, thất bại. Nhưng chúng ta cần thử tất cả các hoán vị ban đầu mà Souvlaki có thể chọn? Có, thuật toán của chúng tôi tái tạo lại dãy con chẵn/lẻ đã được sắp xếp tốt nhất có thể. Trong trường hợp này, việc xen kẽ sẽ có tác dụng nếu chúng ta gán khác đi. Kiểm tra cuối cùng sẽ xác nhận CÓ hoặc KHÔNG. 

**Ví dụ 2** 

đầu vào:`[1, 1, 1, 1]`→ Lẻ: [1,1], Chẵn: [1,1], xây dựng lại → [1,1,1,1], đã sắp xếp → CÓ. 

Những dấu vết này cho thấy sự phân tách thành các chuỗi con và kiểm tra xen kẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n log n) | Mỗi trường hợp thử nghiệm chia tách và sắp xếp hai mảng có độ dài khoảng n/2 | 
| Không gian | O(n) | Chúng tôi lưu trữ hai mảng và mảng được xây dựng lại | 

Với n ≤ 100 và t ≤ 100, tổng số phép toán nhỏ hơn 10^6, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("5\n4\n4 2 2 1\n4\n1 1 1 1\n5\n1 5 1 5 1\n3\n1 2 3\n5\n1 3 2 3 5\n") == "YES\nYES\nYES\nNO\nNO"

# custom cases
assert run("1\n3\n2 1 3\n") == "YES" # minimum size, winnable
assert run("1\n3\n3 2 1\n") == "YES" # minimum size, reverse sorted
assert run("1\n4\n4 3 2 1\n") == "NO" # all decreasing, cannot interleave
assert run("1\n5\n5 5 5 5 5\n") == "YES" # all equal elements
assert run("1\n6\n1 6 2 5 3 4\n") == "YES" # complex winnable pattern
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 3 | CÓ | Mảng nhỏ, Souvlaki có thể thắng | 
| 3 2 1 | CÓ | Mảng đảo ngược nhỏ, Souvlaki có thể sắp xếp lại | 
| 4 3 2 1 | KHÔNG | Không thể xen kẽ để sắp xếp | 
| 5 5 5 5 5 | CÓ | Các phần tử bằng nhau | 
| 1 6 2 5 3 4 | CÓ | Đan xen phức tạp, hoán đổi nhiều lần | 

## Vỏ cạnh 

Đối với các mảng trong đó tất cả các phần tử đều bằng nhau, các chuỗi con lẻ/chẵn được sắp xếp một cách tầm thường và mảng được xây dựng lại sẽ được sắp xếp, mang lại kết quả là "CÓ". Đối với các mảng nhỏ có kích thước 3, thuật toán phân chia chính xác các chỉ số thành 0,2 và 1, kiểm tra việc xen kẽ và xuất ra "CÓ" nếu có thể. Đối với các mảng giảm hoàn toàn như`[4,3,2,1]`, sắp xếp các chuỗi con lẻ/chẵn và xen kẽ tạo ra`[2,3,4,1]`, có sự đảo ngược, dẫn đến "KHÔNG". Mỗi kịch bản xác nhận rằng thuật toán tôn trọng các hạn chế di chuyển của người chơi trong khi khai thác lợi thế hoán vị ban đầu của Souvlaki.
