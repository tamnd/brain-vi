---
title: "CF 2063B - Cập nhật trình tự"
description: "Chúng ta được cung cấp một mảng và một phân đoạn mục tiêu $[l, r]$. Chúng tôi tính toán tổng các phần tử bên trong phân đoạn này, nhưng chúng tôi được phép thực hiện chính xác một thao tác toàn cục trước khi đo nó. Hoạt động này không phải là sự đảo ngược tiêu chuẩn của một mảng con."
date: "2026-06-08T07:27:43+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2063
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1000 (Div. 2)"
rating: 1100
weight: 2063
solve_time_s: 86
verified: true
draft: false
---

[CF 2063B - Cập nhật trình tự](https://codeforces.com/problemset/problem/2063/B) 

**Đánh giá:** 1100 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và một phân khúc mục tiêu$[l, r]$. Chúng tôi tính toán tổng các phần tử bên trong phân đoạn này, nhưng chúng tôi được phép thực hiện chính xác một thao tác toàn cục trước khi đo nó. 

Hoạt động này không phải là sự đảo ngược tiêu chuẩn của một mảng con. Thay vào đó, chúng tôi chọn bất kỳ tập hợp chỉ mục nào theo thứ tự tăng dần và đảo ngược các giá trị được đặt ở các chỉ mục đó với nhau. Nói cách khác, chúng ta lấy một dãy con của các giá trị và chỉ hoán vị những giá trị đã chọn đó bằng cách đảo ngược thứ tự của chúng, trong khi vẫn giữ nguyên mọi thứ khác. 

Mục tiêu là giảm thiểu tổng giá trị nằm trong đoạn cố định$[l, r]$sau sự đảo ngược chuỗi con duy nhất này. 

Quan sát quan trọng là chúng ta không bị hạn chế trong các giao dịch hoán đổi liền kề. Bất kỳ vị trí nào bên ngoài phân khúc đều có thể tương tác với bất kỳ vị trí nào bên trong phân khúc, miễn là chúng ta chọn cả hai chỉ số vào dãy con đã chọn. 

Những ràng buộc đẩy chúng ta ra khỏi bất kỳ cuộc khám phá kết hợp nào. Với$n \le 10^5$mỗi bài kiểm tra và tổng kích thước cũng$10^5$, bất kỳ bậc hai hoặc thậm chí$O(n \log n)$mỗi giải pháp thử nghiệm chỉ được chấp nhận nếu nó đơn giản, nhưng bất kỳ điều gì liên quan đến việc liệt kê tập hợp con hoặc khoảng DP đều không thể thực hiện được ngay lập tức. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị trong phân đoạn đã ở mức tối thiểu hoặc tối đa so với phần còn lại. Một cách tiếp cận hoán đổi tham lam ngây thơ có thể cố gắng liên tục cải thiện phân khúc, nhưng hoạt động này mang tính toàn cầu và chỉ sử dụng một lần, do đó trực giác cập nhật quá mức sẽ không thành công. Một trường hợp cạnh khác là khi$l = r$, trong đó chúng tôi đang giảm thiểu một phần tử; ở đây thao tác chuyển sang lựa chọn xem phần tử đó có thể được thay thế bằng thứ gì đó bên ngoài thông qua ghép nối hay không. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng mô phỏng tất cả các chuỗi có thể xảy ra. Chúng tôi chọn một tập hợp con các chỉ số, đảo ngược giá trị của chúng và tính lại tổng phân đoạn. Ngay cả khi chúng tôi sửa kích thước tập hợp con$k$, việc chọn chỉ số là$\binom{n}{k}$, và tổng hợp tất cả$k$là theo cấp số nhân. Điều này bùng nổ ngay lập tức ngay cả đối với$n = 40$, vì vậy nó không có ý nghĩa gì ngoài tính đúng đắn về mặt khái niệm. 

Cái nhìn sâu sắc về cấu trúc quan trọng là ngừng suy nghĩ về các hoán vị và thay vào đó hãy nghĩ về các cặp đôi. 

Việc đảo ngược một dãy con thực hiện một điều gì đó rất cụ thể: nó tạo ra các cặp chỉ số rời rạc bên trong tập hợp đã chọn, trong đó mỗi cặp hoán đổi các giá trị. Nếu một chỉ mục không được ghép nối, nó sẽ không thay đổi. Vì vậy, hoạt động này tương đương với việc chọn một số giao dịch hoán đổi rời rạc giữa các vị trí. 

Bây giờ chỉ tập trung vào việc tổng phân khúc thay đổi như thế nào. Chỉ những giao dịch hoán đổi vượt qua ranh giới của$[l, r]$vấn đề. Nếu chúng ta hoán đổi hai vị trí cả bên trong hoặc cả bên ngoài thì tổng phân đoạn không thay đổi. Nếu chúng ta hoán đổi một giá trị bên trong với một giá trị bên ngoài, chúng ta sẽ thay thế một giá trị trong phân khúc bằng một giá trị từ bên ngoài một cách hiệu quả. 

Do đó, vấn đề trở thành: chúng ta có thể chọn tối đa$(r-l+1)$các phần tử bên trong phân khúc và khớp chúng với các phần tử bên ngoài, thay thế các giá trị phân khúc bằng các giá trị bên ngoài, nhưng có một ràng buộc: mỗi phần tử bên ngoài có thể được sử dụng nhiều nhất một lần và mỗi lần hoán đổi sẽ tiêu thụ một chỉ mục bên trong và một chỉ mục bên ngoài. 

Để giảm thiểu tổng, chúng tôi muốn thay thế các giá trị lớn nhất trong phân khúc bằng các giá trị nhỏ nhất bên ngoài. Điều này gợi ý việc sắp xếp các ứng cử viên: lấy tất cả các giá trị phân khúc và tất cả các giá trị bên ngoài, đồng thời khớp một cách tham lam giữa phân khúc lớn với phân khúc nhỏ bất cứ khi nào nó cải thiện tổng. 

Chúng tôi không cần cấu trúc khớp đầy đủ ngoài việc ghép nối tham lam này vì mỗi lần hoán đổi có lợi đều có hiệu lực độc lập đối với tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê trình tự Brute Force | hàm mũ | O(n) | Quá chậm | 
| Tham lam ghép đôi sau chia ly | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia mảng thành hai tập hợp: các giá trị bên trong$[l, r]$và các giá trị bên ngoài nó. Tổng phân đoạn được tính từ tập hợp bên trong. 
2. Sắp xếp các giá trị bên trong theo thứ tự giảm dần để chúng ta có thể xem xét các phần tử lớn nhất trước tiên. Đây là những cái duy nhất đáng thay thế vì việc thay thế những cái nhỏ hơn không bao giờ cải thiện tổng hiệu quả hơn việc thay thế những cái lớn hơn. 
3. Sắp xếp các giá trị bên ngoài theo thứ tự tăng dần để trước tiên chúng ta có thể xem xét các giá trị thay thế nhỏ nhất có sẵn. Đây là những ứng viên tốt nhất để đưa vào phân khúc. 
4. Lặp lại cả hai danh sách cùng một lúc. Hãy thử ghép giá trị bên trong lớn nhất còn lại với giá trị bên ngoài nhỏ nhất còn lại. 
5. Chỉ thực hiện hoán đổi nếu giá trị bên ngoài nhỏ hơn giá trị bên trong. Điều kiện này đảm bảo rằng tổng phân đoạn giảm; nếu không thì việc hoán đổi sẽ có hại hoặc trung tính và không nên sử dụng. 
6. Mỗi lần ghép nối thành công sẽ giảm tổng phân đoạn bằng chênh lệch giữa giá trị bên trong và bên ngoài và cả hai con trỏ đều tiến lên. 
7. Dừng lại khi một trong hai danh sách đã hết hoặc không thể cải thiện thêm được nữa. 

Câu trả lời cuối cùng là tổng phân khúc ban đầu trừ đi tất cả những cải tiến thu được từ việc hoán đổi thành công. 

### Tại sao nó hoạt động 

Hoạt động này cho phép ghép nối tùy ý thông qua đảo ngược chuỗi con, do đó, mọi thay đổi hiệu quả đối với phân đoạn đều tương đương với việc trao đổi giá trị giữa chỉ mục bên trong và chỉ mục bên ngoài. Vì mỗi phần tử tham gia vào nhiều nhất một lần hoán đổi nên vấn đề giảm xuống còn việc lựa chọn các trao đổi có lợi rời rạc. 

Chiến lược tham lam là đúng vì mỗi lần hoán đổi đều độc lập trong đóng góp của nó vào tổng thay đổi số tiền: một lần hoán đổi luôn thay đổi số tiền bằng một chênh lệch cố định. Việc kết hợp lợi ích lớn nhất hiện có với chi phí khả dụng nhỏ nhất không bao giờ cản trở một lựa chọn tốt hơn trong tương lai, vì việc sử dụng giá trị bên ngoài lớn hơn thay cho giá trị nhỏ hơn sẽ làm xấu đi hoặc không cải thiện được kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, l, r = map(int, input().split())
        a = list(map(int, input().split()))
        
        l -= 1
        r -= 1
        
        inside = a[l:r+1]
        outside = a[:l] + a[r+1:]
        
        inside.sort(reverse=True)
        outside.sort()
        
        base = sum(inside)
        
        i = 0
        j = 0
        
        while i < len(inside) and j < len(outside):
            if outside[j] < inside[i]:
                base += outside[j] - inside[i]
                i += 1
                j += 1
            else:
                break
        
        print(base)

if __name__ == "__main__":
    solve()
```Việc triển khai phân tách mảng rõ ràng thành các phân đoạn bên trong và bên ngoài, đảm bảo chúng ta chỉ suy luận về các giá trị chứ không phải vị trí. Việc sắp xếp cả hai nhóm là cần thiết vì nó biến quyết định ghép đôi thành một quá trình đơn điệu. 

Một điểm tinh tế là sự bất đẳng thức chặt chẽ trong điều kiện hoán đổi. Cho phép bình đẳng sẽ không cải thiện tổng số và có thể tiêu thụ không chính xác các phần tử hữu ích. 

Một chi tiết quan trọng khác là chúng ta không bao giờ cần phải xây dựng lại mảng. Chỉ có cấu trúc nhiều tập hợp mới quan trọng vì hoạt động đảo ngược chuỗi tiếp theo không duy trì các ràng buộc về vị trí vượt quá khả năng ghép nối. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó$a = [1, 2, 3]$Và$[l, r] = [2, 3]$. 

Bên trong là$[2, 3]$, bên ngoài là$[1]$. Sắp xếp cho bên trong$[3, 2]$, ngoài$[1]$. 

| Bước | Bên trong | Bên ngoài | Hành động | Tổng phân đoạn | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [3, 2] | [1] | không | 5 | 
| 1 | [3, 2] | [1] | hoán đổi 3 với 1 | 3 | 

Thuật toán thực hiện một lần hoán đổi có lợi, thay 3 bằng 1, giảm tổng từ 5 xuống 3. Điều này phù hợp với cấu hình tối ưu vì chỉ tồn tại một phần tử bên ngoài. 

Bây giờ hãy xem xét$a = [5, 1, 4, 2]$với$[l, r] = [2, 3]$. 

Bên trong là$[1, 4]$, bên ngoài là$[5, 2]$. Sắp xếp cho bên trong$[4, 1]$, ngoài$[2, 5]$. 

| Bước | Bên trong | Bên ngoài | Hành động | Tổng phân đoạn | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [4, 1] | [2, 5] | không | 5 | 
| 1 | [4, 1] | [2, 5] | hoán đổi 4 với 2 | 3 | 
| dừng lại | [1] | [5] | 2 ≥ 1 không hoán đổi | 3 | 

Dấu vết này cho thấy tại sao việc dừng tham lam là cần thiết: một khi ứng cử viên bên ngoài tốt nhất có sẵn không nhỏ hơn giá trị bên trong hiện tại thì không thể cải thiện thêm nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp mảng bên trong và bên ngoài chiếm ưu thế | 
| Không gian |$O(n)$| lưu trữ mảng phân chia | 

Tổng kích thước trên các trường hợp thử nghiệm là$10^5$, do đó việc sắp xếp theo từng trường hợp thử nghiệm là đủ hiệu quả trong Python. Thuật toán chỉ thực hiện các bước tuyến tính sau khi sắp xếp, giữ các hệ số không đổi ở mức nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, l, r = map(int, input().split())
        a = list(map(int, input().split()))
        l -= 1
        r -= 1

        inside = a[l:r+1]
        outside = a[:l] + a[r+1:]

        inside.sort(reverse=True)
        outside.sort()

        s = sum(inside)
        i = j = 0
        while i < len(inside) and j < len(outside):
            if outside[j] < inside[i]:
                s += outside[j] - inside[i]
                i += 1
                j += 1
            else:
                break

        out.append(str(s))
    return "\n".join(out)

# provided samples
assert run("""6
2 1 1
2 1
3 2 3
1 2 3
3 1 3
3 1 2
4 2 3
1 2 2 2
5 2 5
3 3 2 3 5
6 1 3
3 6 6 4 3 2
""") == """1
3
6
3
11
8"""

# custom cases
assert run("""1
1 1 1
100
""") == "100"

assert run("""1
5 1 5
5 4 3 2 1
""") == "15"

assert run("""1
5 2 4
1 100 1 100 1
""") == "3"

assert run("""1
4 2 3
10 1 2 3
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 100 | trường hợp ranh giới tối thiểu | 
| đoạn đảo ngược hoàn toàn | 15 | không thể thực hiện hoán đổi có lợi | 
| xen kẽ các thái cực | 3 | tham lam ghép nối đúng đắn | 
| cửa sổ nhỏ ở giữa | 3 | tương tác phân chia bên trong/bên ngoài | 

## Vỏ cạnh 

Khi phân đoạn là một phần tử, thuật toán sẽ giảm xuống để kiểm tra xem có giá trị bên ngoài nào nhỏ hơn hay không. Logic sắp xếp vẫn hoạt động, nhưng chỉ tồn tại một ứng cử viên bên trong, vì vậy có thể có nhiều nhất một sự thay thế. Dấu vết là tầm thường và phù hợp với trực giác. 

Khi tất cả các phần tử đều bằng nhau, việc sắp xếp sẽ tạo ra các danh sách giống hệt nhau. Điều kiện hoán đổi không thành công ngay lập tức, do đó không có thay đổi nào được áp dụng và số tiền ban đầu được trả về. Điều này đúng vì không có thao tác nào có thể cải thiện các giá trị đồng nhất. 

Khi đoạn này nằm ở ranh giới, chẳng hạn như$l = 1$hoặc$r = n$, một trong các danh sách sẽ trống. Thuật toán thực hiện hoán đổi bằng 0 một cách tự nhiên và câu trả lời vẫn là tổng của phân khúc vì không có tài liệu bên ngoài để trao đổi.
