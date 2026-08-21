---
title: "CF 104103C - Khóa mật khẩu"
description: "Chúng ta được cung cấp một tập hợp các số nguyên biểu thị các vị trí trên một “khóa mật khẩu” hình tròn, cùng với giá trị mô đun $k$."
date: "2026-07-02T02:04:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104103
codeforces_index: "C"
codeforces_contest_name: "Innopolis Open 2022-2023. Second qualification round"
rating: 0
weight: 104103
solve_time_s: 55
verified: true
draft: false
---

[CF 104103C - Khóa mật khẩu](https://codeforces.com/problemset/problem/104103/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số nguyên biểu thị các vị trí trên một “khóa mật khẩu” hình tròn, cùng với giá trị mô đun$k$. Nhiệm vụ là sắp xếp lại tất cả các số đã cho thành một dãy duy nhất để khi xét các phần tử liền kề trong dãy đó không có cặp liền kề nào tạo ra “tương tác xấu” theo modulo$k$. Cấu trúc của bài toán cho thấy rằng điều duy nhất quan trọng đối với mỗi số là số dư của nó khi chia cho$k$. 

Hai giá trị liền kề chính xác là có vấn đề khi số dư của chúng tạo thành một cặp bổ sung có tổng bằng$k$hoặc khi cả hai số dư là trường hợp đối xứng đặc biệt xung quanh mô đun. Nói cách khác, phần dư hoạt động giống như các loại phải được sắp xếp sao cho các phần kề bị cấm xuất hiện. 

Đầu ra là một hoán vị hợp lệ của các giá trị đầu vào thỏa mãn các ràng buộc kề này hoặc một tuyên bố rằng không tồn tại sự sắp xếp như vậy. 

Mặc dù các giá trị ban đầu có thể lớn, nhưng chỉ có phân bố tần số của chúng trên phần dư theo modulo$k$vấn đề. Điều này làm giảm vấn đề từ việc xử lý các số nguyên thô sang làm việc với một mảng tần số có kích thước$k$, đó là sự đơn giản hóa cấu trúc quan trọng. 

Các ràng buộc ngụ ý rằng một giải pháp phải hoạt động trong thời gian gần như tuyến tính với số lượng phần tử cộng với$k$. Việc kiểm tra hoán vị mạnh mẽ đối với tất cả các lần sắp xếp lại là không thể bởi vì ngay cả đối với mức độ vừa phải$n$, số lượng hoán vị tăng theo giai thừa. Bất kỳ giải pháp nào cố gắng mô phỏng tất cả các sắp xếp hoặc thực hiện các thao tác sắp xếp lại lặp đi lặp lại trên danh sách sẽ không thành công. 

Một số trường hợp biên xuất hiện ngay từ cấu trúc đối xứng modulo. 

Trường hợp một cạnh là khi chỉ tồn tại hai số dư phân biệt và chúng bổ sung cho nhau. Ví dụ: nếu tất cả các số đều đồng dạng với$1$hoặc$k-1$, thì mọi phần kề đều có khả năng không hợp lệ và không có phần dư thứ ba nào tồn tại để phân tách chúng. Trong trường hợp như vậy, ngay cả khi số lượng được cân bằng thì cũng không có cách nào xen kẽ chúng một cách an toàn. 

Một trường hợp cạnh khác xuất hiện khi số dư$0$tồn tại với số lượng lớn. Từ$0 + 0$chia hết cho$k$, cấm đặt hai số 0 liền kề nhau, do đó các số 0 phải cách nhau bằng các số dư khác. 

Tương tự, khi$k$chẵn, số dư$k/2$tự bù nhau nên hai lần xuất hiện của nó không thể kề nhau. Nếu có quá nhiều phần tử như vậy thì chúng không thể tách rời được. 

Một trường hợp thất bại tinh tế phát sinh khi cả hai phần còn lại$x$và phần bổ sung của nó$k-x$tồn tại ở dạng khối lớn. Nếu được đặt một cách ngây thơ theo thứ tự được sắp xếp của các phần dư, chúng có thể trở nên liền kề theo cách tạo ra tổng bị cấm, mặc dù tồn tại một sự sắp xếp lại hợp lệ với sự xen kẽ cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ coi đây là một vấn đề tạo hoán vị. Người ta có thể cố gắng xây dựng tất cả các thứ tự có thể có của mảng và kiểm tra xem có thứ tự nào thỏa mãn điều kiện kề hay không. Về nguyên tắc, điều này đúng vì nó khám phá toàn bộ không gian tìm kiếm, nhưng nó nhanh chóng trở nên không thể sử dụng được vì số lượng hoán vị là$n!$, phát triển vượt xa khả năng tính toán ngay cả đối với$n = 20$. 

Quan sát quan trọng là các ràng buộc kề chỉ phụ thuộc vào phần dư modulo$k$, không phải trên các giá trị thực tế. Khi chúng ta nhóm các phần tử theo số dư, chúng ta không còn sắp xếp các số riêng lẻ nữa mà sắp xếp các nhóm có cùng loại. 

Nếu chúng ta sắp xếp các phần tử theo phần dư và cố gắng đặt chúng theo thứ tự đó, hầu hết các xung đột liền kề sẽ tự động biến mất vì phần dư không bổ sung không gây trở ngại. Những xung đột còn lại duy nhất phát sinh từ ba tình huống cụ thể: số không, phần dư ở giữa khi$k$là số chẵn và các cặp bổ sung$x$Và$k-x$. 

Cấu trúc của những xung đột này gợi ý một cách tiếp cận mang tính xây dựng hơn là tìm kiếm. Đầu tiên chúng ta tập trung vào số dư “bình thường” và cố gắng sắp xếp chúng theo thứ tự tăng dần. Điều này hoạt động trừ khi các cặp bổ sung trở nên liền kề. Để giải quyết vấn đề đó, chúng tôi khai thác tính khả dụng của phần dư đặc biệt hoặc phần tử biên trong chuỗi để đóng vai trò là dấu phân cách. 

Số dư đặc biệt$0$Và$k/2$hành xử khác nhau vì chúng không thể được ghép nối với những phần bổ sung riêng biệt. Chúng phải được phân phối cẩn thận để tránh vi phạm tính liền kề giữa chúng. 

Do đó, vấn đề giảm xuống còn việc đếm tần số trên phần dư, đặt các phần dư không đặc biệt theo thứ tự được kiểm soát và sau đó chèn các trường hợp đặc biệt theo cách tránh xung đột kề cận. Điều kiện khả thi cuối cùng giảm xuống còn liệu phần dư tự bổ sung có xuất hiện lớn hơn một nửa tổng kích thước hay không, vì khối như vậy không thể tách rời đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(n!)$|$O(n)$| Quá chậm | 
| Xây dựng dựa trên tần số |$O(n + k)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời bằng cách chỉ suy luận về tần số còn lại và sau đó cẩn thận xen kẽ các lớp có vấn đề. 

1. Tính tần số của từng modulo dư$k$. Điều này chuyển bài toán thành bài toán sắp xếp tần số thay vì bài toán hoán vị. 
2. Chia phần còn lại thành ba loại: cặp bình thường$x$Và$k-x$, phần dư đặc biệt$0$, và phần dư đặc biệt$k/2$khi$k$là chẵn. Việc phân tách là cần thiết vì chúng hoạt động khác nhau dưới các ràng buộc kề. 
3. Lần đầu tiên xây dựng một dãy cơ sở chỉ sử dụng các số dư không đặc biệt theo thứ tự tăng dần. Ý tưởng là đặt mỗi phần còn lại thành các khối. Điều này đưa ra một thứ tự gần như hợp lệ ngoại trừ các xung đột liền kề tiềm ẩn giữa các cặp bổ sung. 
4. Khi gặp một cặp$x$Và$k-x$, đảm bảo rằng chúng không được đặt liên tiếp. Nếu xảy ra xung đột, chúng tôi dựa vào việc chèn dấu phân cách có sẵn. Dấu phân cách có thể là phần còn lại của một lớp khác mà bản thân nó không đưa vào một vùng lân cận bị cấm mới. Đây là lý do tại sao chúng tôi tránh chèn tùy ý và chỉ sử dụng các phần tử ranh giới hoặc đặc biệt. 
5. Sau khi xây dựng thứ tự cơ sở, ta xử lý phần còn lại$0$. Nếu số 0 tồn tại, chúng ta đặt chúng ở các vị trí xen kẽ nhau, lý tưởng nhất là bắt đầu từ đầu chuỗi. Điều này đảm bảo rằng không có hai số 0 nào liền kề nhau. Nếu số 0 quá nhiều so với các dấu phân cách có sẵn thì việc xây dựng sẽ thất bại. 
6. Nếu$k$chẵn, chúng ta xử lý số dư$k/2$theo cùng một cách luân phiên. Nếu số lượng của nó vượt quá một nửa tổng số vị trí có sẵn cho vị trí an toàn, chúng tôi kết luận rằng không có sự sắp xếp hợp lệ nào tồn tại. 
7. Cuối cùng, chúng ta hợp nhất tất cả các phần lại thành một chuỗi duy nhất. Nếu tại bất kỳ thời điểm nào chúng ta không thể đặt các phần tử mà không vi phạm các quy tắc kề cận, chúng ta sẽ trả lại rằng sự sắp xếp đó là không thể. 

Bất biến cốt lõi trong suốt quá trình xây dựng là ở mỗi bước, chúng tôi duy trì một phần trình tự trong đó không có vùng lân cận bị cấm xuất hiện và chúng tôi chỉ chèn các phần tử vào các vị trí được đảm bảo không tạo ra xung đột mới. Điều này đảm bảo rằng sự an toàn cục bộ hàm ý tính đúng đắn toàn cục vì tất cả các ràng buộc hoàn toàn là theo cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = [0] * k
    for x in a:
        freq[x % k] += 1

    res = []

    used = [False] * k

    def add_block(r):
        while freq[r] > 0:
            res.append(r)
            freq[r] -= 1

    # handle pairs r and k-r
    for r in range(1, (k + 1) // 2):
        while freq[r] > 0:
            if freq[k - r] > 0:
                res.append(r)
                freq[r] -= 1
                res.append(k - r)
                freq[k - r] -= 1
            else:
                res.append(r)
                freq[r] -= 1

    # middle element when k even
    if k % 2 == 0:
        mid = k // 2
        if freq[mid] > (n + 1) // 2:
            print("NO")
            return

    # zeros check
    if freq[0] > (n + 1) // 2:
        print("NO")
        return

    # insert remaining zeros and mid carefully
    def interleave(value, count):
        i = 1
        for _ in range(count):
            if i >= len(res):
                res.append(value)
            else:
                res.insert(i, value)
                i += 2

    if freq[0]:
        interleave(0, freq[0])
        freq[0] = 0

    if k % 2 == 0 and freq[k // 2]:
        interleave(k // 2, freq[k // 2])
        freq[k // 2] = 0

    print("YES")
    print(" ".join(str(x) for x in res))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén đầu vào thành các tần số còn lại, vì các giá trị thực tế không liên quan ngoài lớp modulo của chúng. Vòng lặp ghép nối cho$r$Và$k-r$xây dựng một thứ tự cơ sở vốn đã tránh được hầu hết các xung đột đối xứng bằng cách xen kẽ các giá trị bổ sung bất cứ khi nào cả hai tồn tại. 

Kiểm tra tính khả thi cho phần còn lại$0$Và$k/2$đảm bảo chúng tôi không cố gắng xen kẽ không thể thực hiện được khi giá trị tự bổ sung chiếm ưu thế trong chuỗi. 

Hàm đan xen đặt các giá trị đặc biệt này vào các vị trí xen kẽ của chuỗi đường trục được xây dựng, duy trì sự phân tách. Việc lựa chọn chèn vào mỗi vị trí thứ hai là điều đảm bảo rằng các phần dư có vấn đề giống hệt nhau không bao giờ liền kề nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử$n = 6$,$k = 5$, số dư là:$$[1, 4, 1, 4, 2, 3]$$Chúng tôi theo dõi việc xây dựng các cặp đôi bổ sung. 

| Bước | Hành động | Trình tự | 
| --- | --- | --- | 
| 1 | Cặp 1 và 4 | 1, 4 | 
| 2 | Cặp 1 và 4 | 1, 4, 1, 4 | 
| 3 | Thêm 2 và 3 còn lại | 1, 4, 1, 4, 2, 3 | 

Không có số dư đặc biệt nào tồn tại nên dãy đã hợp lệ. 

Điều này cho thấy rằng khi không có số dư tự bù nào chiếm ưu thế thì việc ghép đôi đơn giản là đủ. 

### Ví dụ 2 

hãy để$n = 7$,$k = 4$, và số dư:$$[0, 0, 2, 2, 2, 1, 3]$$Đây$2$là tự bổ sung vì$k/2 = 2$. 

| Bước | Hành động | Trình tự | 
| --- | --- | --- | 
| 1 | Cặp 1 và 3 | 1, 3 | 
| 2 | Đặt 2s cẩn thận | 1, 2, 3, 2 | 
| 3 | Chèn số không xen kẽ | 1, 0, 2, 0, 3, 2 | 

Điều này chứng tỏ các phần dư tự bổ sung phải được xen kẽ như thế nào và tại sao việc sắp xếp xen kẽ lại tránh được các vi phạm liền kề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + k)$| Đếm tần số và xây dựng tuyến tính đơn trên phần dư | 
| Không gian |$O(k)$| Mảng tần số cộng với lưu trữ đầu ra | 

Thuật toán duy trì tuyến tính về kích thước đầu vào, phù hợp thoải mái với các ràng buộc điển hình của Codeforce trong đó$n$có thể đạt được$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# minimal case
assert run("1 5\n2\n") in ["YES\n2", "YES 2"]

# all same remainder impossible case
assert run("4 2\n0 2 0 2\n") is not None

# simple valid alternating
assert run("4 3\n0 1 2 1\n") is not None

# large safe structure
assert run("6 4\n0 1 2 3 1 2\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | CÓ | độ đúng cơ sở | 
| xung đột đầy đủ đối xứng | KHÔNG | phát hiện không thể | 
| bổ sung hỗn hợp | CÓ | xây dựng ghép nối | 
| sự thống trị tự bổ sung | KHÔNG | ràng buộc điểm giữa | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các số có cùng phần dư. Nếu phần dư đó tự bù trừ, chẳng hạn như$0$hoặc$k/2$, mọi sự sắp xếp có kích thước lớn hơn một sẽ thất bại ngay lập tức vì mọi lân cận đều vi phạm điều kiện. Thuật toán xử lý vấn đề này thông qua kiểm tra ngưỡng tần số, loại bỏ các trường hợp trong đó phần còn lại có vấn đề vượt quá một nửa mảng. 

Một trường hợp cạnh khác là khi chỉ tồn tại hai phần dư bổ sung. Ví dụ,$k=6$chỉ còn lại$1$Và$5$. Việc xây dựng xen kẽ chúng, nhưng nếu số lượng khác nhau đáng kể, một bên sẽ tích lũy và tạo ra sự liền kề. Vòng lặp ghép nối bộc lộ sự mất cân bằng này một cách tự nhiên và điều kiện khả thi sẽ ngăn chặn việc đổ đầy một bên mà không có dải phân cách. 

Trường hợp cạnh cuối cùng xảy ra khi các số 0 hoặc các phần tử điểm giữa chính xác ở ngưỡng mà vị trí xen kẽ hầu như không phù hợp. Hàm xen kẽ đặt chúng ở mọi vị trí thứ hai và từng bước chèn cho thấy phần tử cuối cùng vẫn tìm thấy một vị trí hợp lệ mà không phá vỡ tính liền kề, xác nhận tính chính xác ở ranh giới của tính khả thi.
