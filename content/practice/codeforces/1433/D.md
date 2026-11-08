---
title: "CF 1433D - Kết nối các quận"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản có $n$ quận và mỗi quận thuộc về một nhóm nào đó được xác định bằng nhãn số nguyên."
date: "2026-06-11T04:58:17+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar"]
categories: ["algorithms"]
codeforces_contest: 1433
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 677 (Div. 3)"
rating: 1200
weight: 1433
solve_time_s: 88
verified: false
draft: false
---

[CF 1433D - Kết nối các quận](https://codeforces.com/problemset/problem/1433/D) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, dfs và tương tự 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi tình huống đều có$n$các quận và mỗi quận thuộc về một nhóm nào đó được xác định bằng một nhãn số nguyên. Nhiệm vụ của chúng ta là xây dựng một tập hợp chính xác$n-1$đường không có định hướng để tất cả các quận được kết nối như một thành phần được kết nối duy nhất, nghĩa là có thể đến bất kỳ quận nào từ bất kỳ quận nào khác thông qua những con đường này. 

Hạn chế mang tính cấu trúc: chúng tôi không được phép kết nối trực tiếp hai quận nếu chúng thuộc cùng một băng nhóm. Mỗi cạnh trong đồ thị cuối cùng phải nối hai đỉnh có nhãn khác nhau. Chúng ta có thể tự do lựa chọn bất kỳ cấu trúc cây nào miễn là nó trải dài trên tất cả các đỉnh và tôn trọng ràng buộc này. Nếu không có cây như vậy tồn tại, chúng tôi phải thông báo là không thể có được. 

Ràng buộc$\sum n \le 5000$có nghĩa là trên tất cả các trường hợp thử nghiệm, chúng tôi có thể cung cấp các giải pháp gần như tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ phương trình bậc hai nào trong trường hợp xấu nhất cho mỗi trường hợp thử nghiệm vẫn sẽ đạt, nhưng bất kỳ phương trình bậc hai nào hoặc liên quan đến việc tính toán lại tốn kém nhiều lần sẽ trở nên không an toàn nếu lặp lại qua nhiều thử nghiệm. Điều này thúc đẩy chúng tôi hướng tới giải pháp dựa trên xây dựng thay vì tìm kiếm hoặc quay lại. 

Một trường hợp quan trọng xuất hiện khi tất cả các quận thuộc cùng một nhóm. Trong tình huống đó, mọi cặp có thể đều vi phạm ràng buộc, do đó, ngay cả cạnh đầu tiên cũng không thể được thêm vào. 

Một trường hợp tinh tế khác xảy ra khi có chính xác hai nhóm nhưng một nhóm chỉ có một nút duy nhất. Ví dụ: nếu chúng tôi cố gắng kết nối các nút một cách tham lam trong một cụm màu duy nhất, chúng tôi có thể cô lập đơn vị đó và chỉ có chiến lược kết nối chéo cẩn thận mới duy trì được kết nối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng một cây bao trùm hợp lệ bằng cách kiểm tra tất cả các cây có thể hoặc xây dựng dần dần các cạnh trong khi xác minh khả năng kết nối và ràng buộc màu sắc. Về mặt khái niệm, chúng ta có thể thử DFS hoặc BFS trên tất cả các cạnh tuân thủ quy tắc và xem liệu chúng ta có thể tiếp cận tất cả các nút hay không, nhưng điều đó bỏ qua yêu cầu là chúng ta phải xuất chính xác$n-1$các cạnh tạo thành cây. Việc liệt kê các cây ứng cử viên là theo cấp số nhân trong$n$, vì số lượng cây được dán nhãn là$n^{n-2}$và thậm chí lọc chúng theo giới hạn màu sắc là không khả thi. 

Cái nhìn sâu sắc về cấu trúc quan trọng là chúng ta không cần tìm kiếm một cái cây; chúng ta chỉ cần xây dựng một. Trở ngại duy nhất là các cạnh phải luôn kết nối các nhóm khác nhau. Nếu tồn tại ít nhất một nút của một nhóm khác với nút gốc đã chọn thì nút gốc đó có thể đóng vai trò là trung tâm: mọi nút khác có thể được gắn vào nút đó hoặc với nút đại diện được chọn cẩn thận, đảm bảo kết nối mà không vi phạm ràng buộc. 

Lần duy nhất điều này không thành công là khi tất cả các nút thuộc cùng một nhóm, bởi vì khi đó không có cạnh hợp lệ nào tồn tại. 

Điều này gợi ý một chiến lược mang tính xây dựng đơn giản: chọn một nút làm tham chiếu, tốt nhất là từ đa số hoặc đơn giản là chỉ mục 1, sau đó kết nối mọi nút của một nhóm khác với nút đó. Tuy nhiên, chỉ điều này không đảm bảo khả năng kết nối giữa các nút có cùng cấu trúc nhóm, vì vậy chúng tôi tinh chỉnh ý tưởng: chúng tôi chọn một nút trục từ một nhóm và đảm bảo rằng tất cả các kết nối được tạo từ trục này hoặc từ các nút của các nhóm khác nhau, xây dựng một cách hiệu quả cấu trúc ngôi sao hoặc gần sao. 

Sau đó, chúng tôi đảm bảo rằng đối với mọi nút có màu khớp với trục, chúng tôi tránh kết nối trực tiếp với trục và thay vào đó kết nối nó thông qua một số nút có màu khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm cây) | Hàm mũ | O(n) | Quá chậm | 
| Chiến lược xoay trục mang tính xây dựng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng một cây bao trùm hợp lệ bằng cách sử dụng cấu trúc đại diện cố định. 

1. Chọn bất kỳ nút nào làm tham chiếu, thường là nút 1 và ghi nhãn nhóm của nó$c$. 
2. Phân chia tất cả các nút thành hai nhóm: những nút có nhãn$c$, và những cái có nhãn không bằng$c$. Sự phân chia này rất quan trọng vì mọi cạnh hợp lệ ít nhất phải vượt qua ranh giới này khi liên quan đến nút tham chiếu. 
3. Nếu không có nút nào có nhãn khác với$c$, xuất ra NO ngay lập tức. Điều này tương ứng với trường hợp mọi cạnh đều vi phạm ràng buộc. 
4. Chọn một nút$j$sao cho nhãn của nó khác với$c$. Nút này hoạt động như một đầu nối phổ quát. 
5. Kết nối nút 1 với nút$j$. Điều này đảm bảo có ít nhất một cầu nối hợp lệ giữa hai nhóm. 
6. Đối với mọi nút còn lại$i > 1$, kết nối nó với nút 1 nếu nhãn của nó khác với$c$. Nếu không, hãy kết nối nó với nút$j$. 

Lý do phân chia quy tắc đính kèm là nút 1 không thể kết nối an toàn với các nút có màu riêng của nó, nhưng nút$j$có thể hoạt động như một mỏ neo thay thế cho nhóm màu đó. 

### Tại sao nó hoạt động 

Việc xây dựng đảm bảo rằng mỗi nút được kết nối bằng chính xác một cạnh, ngoại trừ hai điểm neo được kết nối với nhau, tạo thành một cây có$n-1$các cạnh. Khả năng kết nối tuân theo vì mọi nút đều được gắn trực tiếp vào nút 1 hoặc nút$j$, và hai mỏ neo đó được kết nối. Mọi cạnh đều hợp lệ vì chúng tôi chỉ kết nối các nút có màu khác nhau bằng cách xây dựng. Cấu trúc không theo chu kỳ vì mọi nút ngoại trừ hai nút neo đều có chính xác một nút cha và hai nút neo tạo thành liên kết chéo duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        root_color = a[0]
        other = -1

        for i in range(n):
            if a[i] != root_color:
                other = i
                break

        if other == -1:
            print("NO")
            continue

        print("YES")

        # connect root (0) with first different color node
        print(1, other + 1)

        # attach remaining nodes
        for i in range(1, n):
            if i == other:
                continue
            if a[i] == root_color:
                print(other + 1, i + 1)
            else:
                print(1, i + 1)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng việc xác định xem băng đảng thứ hai có tồn tại hay không. Biến`other`lưu trữ chỉ mục đầu tiên với nhãn khác với nút đầu tiên. Nếu nó vẫn còn`-1`, tất cả các nút chia sẻ cùng một nhóm và không có giải pháp nào tồn tại. 

Sau đó, việc xây dựng cạnh sẽ cố định nút 1 làm trung tâm chính. Nút liên nhóm đầu tiên trở thành trung tâm thứ cấp. Mọi nút đều gắn vào nút 1 hoặc với trung tâm phụ này tùy thuộc vào việc nó có khớp với nhóm của nút 1 hay không. Điều này tránh việc tạo ra một cạnh giống nhau trong khi vẫn đảm bảo tất cả các nút được kết nối qua tối đa hai đỉnh trung tâm. 

Một chi tiết tinh tế được bỏ qua`other`nút trong vòng lặp vì nó đã tham gia vào cạnh kết nối ban đầu. Nếu không bỏ qua bước này, chúng ta sẽ tạo một cấu trúc trùng lặp hoặc tự tham chiếu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [1, 2, 2, 1, 3]
```Chúng tôi chọn nút 1 làm gốc với màu 1. Nút khác đầu tiên là chỉ số 2 (màu 2). 

| Bước | Hành động | Cạnh cho đến nay | 
| --- | --- | --- | 
| 1 | kết nối 1 và 2 | (1,2) | 
| 2 | nút 3 có màu 2, gắn vào 1 | (1,2), (1,3) | 
| 3 | nút 4 có màu 1, gắn vào 2 | (1,2), (1,3), (2,4) | 
| 4 | nút 5 có màu 3, gắn vào 1 | (1,2), (1,3), (2,4), (1,5) | 

Điều này tạo thành một cây hợp lệ trong đó tất cả các cạnh kết nối các nhóm khác nhau và tất cả các nút đều có thể truy cập được thông qua nút 1 hoặc nút 2. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [7, 7, 7, 7]
```Tất cả các nút chia sẻ cùng một nhãn. Thuật toán phát hiện rằng không có nút nào khác với nút đầu tiên và ngay lập tức xuất ra NO. Bất kỳ cạnh nào được cố gắng thực hiện sẽ vi phạm ràng buộc, vì vậy việc chấm dứt sớm là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét mảng một lần để tìm nút màu khác và sau đó xuất ra chính xác$n-1$cạnh | 
| Không gian | O(1) thêm | Chỉ một số chỉ mục được lưu trữ ngoài mảng đầu vào | 

Tổng số tiền của$n$được giới hạn bởi 5000, do đó việc quét tuyến tính trên mỗi trường hợp thử nghiệm dễ dàng đủ nhanh trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        root_color = a[0]
        other = -1
        for i in range(n):
            if a[i] != root_color:
                other = i
                break

        if other == -1:
            out.append("NO")
            continue

        edges = []
        edges.append((1, other + 1))

        for i in range(1, n):
            if i == other:
                continue
            if a[i] == root_color:
                edges.append((other + 1, i + 1))
            else:
                edges.append((1, i + 1))

        out.append("YES")
        for u, v in edges:
            out.append(f"{u} {v}")

    return "\n".join(out)

# provided sample
assert run("""4
5
1 2 2 1 3
3
1 1 1
4
1 1000 101 1000
4
1 2 3 4
""") == """YES
1 2
1 3
2 4
1 5
NO
YES
1 2
2 3
3 4
YES
1 2
1 3
1 4"""

# all equal minimum
assert run("""1
2
5 5
""") == """NO"""

# minimal valid
assert run("""1
2
1 2
""") == """YES
1 2"""

# star-like case
assert run("""1
4
1 2 3 4
""") in ["YES\n1 2\n1 3\n1 4", "YES\n1 2\n1 3\n1 4"]

# two groups skewed
assert run("""1
5
1 1 1 2 2
""").startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | KHÔNG | phát hiện không thể | 
| tối thiểu hai nút khác nhau | CÓ cạnh | xây dựng căn cứ | 
| nhãn hoàn toàn đa dạng | cấu trúc sao | tính đúng đắn của logic đính kèm | 
| phân phối lệch | kết nối | sự mạnh mẽ của sự lựa chọn trung tâm | 

## Vỏ cạnh 

Khi tất cả các quận thuộc cùng một nhóm, quá trình quét sẽ không bao giờ tìm thấy nút phụ hợp lệ, do đó thuật toán sẽ ngay lập tức đưa ra NO. Đối với đầu vào như`1 1 1 1`, điều này phản ánh chính xác rằng không có lợi thế pháp lý nào tồn tại cả. 

Khi có chính xác hai nhóm tồn tại nhưng một nhóm chỉ xuất hiện một lần, nút phụ sẽ trở thành cầu nối quan trọng. Ví dụ,`1 1 1 2`, nút 2 trở thành trình kết nối an toàn duy nhất cho tất cả các nút có màu 1. Việc xây dựng định tuyến tất cả các nút cùng màu thông qua nút có màu đối diện, đảm bảo không có cạnh không hợp lệ nào được tạo trong khi vẫn tạo ra cây được kết nối.
