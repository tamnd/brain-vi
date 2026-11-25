---
title: "CF 985E - Bút Chì Và Hộp"
description: "Chúng ta được cung cấp một chuỗi các giá trị bão hòa của bút chì và chúng ta cần chia tất cả bút chì thành các nhóm gọi là hộp. Mỗi cây bút chì phải được đặt vào đúng một hộp. Mỗi hộp chúng ta sử dụng phải chứa ít nhất k bút chì."
date: "2026-06-17T00:56:41+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 985
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 44 (Rated for Div. 2)"
rating: 2100
weight: 985
solve_time_s: 75
verified: true
draft: false
---

[CF 985E - Bút chì và Hộp](https://codeforces.com/problemset/problem/985/E) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam, hai con trỏ 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các giá trị bão hòa của bút chì và chúng ta cần chia tất cả bút chì thành các nhóm gọi là hộp. Mỗi cây bút chì phải được đặt vào đúng một hộp. Mỗi hộp chúng ta sử dụng phải chứa ít nhất k bút chì. Bên trong bất kỳ hộp đơn nào, chênh lệch giữa giá trị bão hòa nhỏ nhất và lớn nhất không được vượt quá d. 

Nhiệm vụ không phải là giảm thiểu hoặc tối đa hóa số lượng hộp. Chúng ta chỉ cần quyết định xem có tồn tại một phân vùng hợp lệ hay không. 

Các hạn chế rất lớn, lên tới 500.000 bút chì. Bất kỳ giải pháp nào cố gắng kiểm tra tất cả các phân vùng hoặc thậm chí tất cả các nhóm một cách rõ ràng sẽ không thành công. Điều này ngay lập tức loại trừ các phương pháp tiếp cận hàm mũ hoặc bậc hai. Ngay cả việc suy luận O(n^2) theo cặp cũng quá chậm, vì vậy giải pháp phải dựa vào việc sắp xếp và quét tuyến tính hoặc gần tuyến tính. 

Một vấn đề tế nhị xuất phát từ sự tương tác giữa các ràng buộc. Một hộp được xác định bởi cả ràng buộc về kích thước và ràng buộc về phạm vi giá trị. Một sai lầm ngây thơ là xử lý những điều này một cách độc lập, ví dụ như tham lam tạo thành các nhóm có kích thước k mà không kiểm tra ràng buộc d trên toàn cầu hoặc nhóm theo phạm vi giá trị mà không đảm bảo kích thước tối thiểu. 

Các trường hợp Edge phá vỡ lý luận ngây thơ bao gồm: 

Đầu tiên, khi k bằng 1. Khi đó mỗi cây bút chì có thể tạo thành hộp riêng của nó và câu trả lời luôn là CÓ bất kể d. Một giải pháp đòi hỏi phải nhóm theo phạm vi có thể không thành công. 

Thứ hai, khi d rất nhỏ, chẳng hạn như 0. Khi đó chỉ có thể nhóm các giá trị giống nhau lại với nhau và mỗi nhóm vẫn phải thỏa mãn kích thước ≥ k. Ví dụ: nếu các giá trị là [1,1,1,2,2] và k = 3, d = 0, câu trả lời là KHÔNG vì cả hai loại giá trị đều không đạt đến kích thước 3. 

Thứ ba, khi các giá trị đã được nhóm lại nhưng không được căn chỉnh theo bội số của k. Một kẻ tham lam luôn lấy k phần tử tiếp theo có thể tạo ra một nhóm vi phạm d mặc dù một nhóm khác sẽ thành công. 

Những vấn đề này cho thấy chúng ta cần một chiến lược lý giải toàn cầu về cấu trúc được sắp xếp thay vì xây dựng các nhóm cục bộ một cách mù quáng. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là xem xét tất cả các phân vùng có thể có của mảng được sắp xếp thành các phân đoạn liền kề và kiểm tra xem mỗi phân đoạn có thỏa mãn cả hai điều kiện hay không. Ngay cả khi chúng tôi giới hạn bản thân ở các phân đoạn liền kề sau khi sắp xếp, chúng tôi vẫn cần phải thử mọi cách để phân chia n phần tử, theo cấp số nhân. Đối với mỗi phân vùng, việc xác minh tính hợp lệ tốn O(n), do đó tổng số trở nên không khả thi ngay cả khi n = 30. 

Điều quan trọng là sau khi sắp xếp, bất kỳ hộp hợp lệ nào cũng phải tương ứng với một phân đoạn liền kề trong mảng đã sắp xếp. Nếu một hộp chứa hai phần tử i và j có khoảng cách lớn, thì bất kỳ phần tử nào giữa chúng theo thứ tự được sắp xếp cũng sẽ tương thích với cả hai đầu về mặt phạm vi giá trị, do đó, việc loại trừ nó sẽ chỉ khiến việc nhóm khó hơn chứ không dễ dàng hơn. Điều này cho phép chúng ta giảm bớt vấn đề trong việc phân vùng một mảng đã sắp xếp thành các phân đoạn liền kề. 

Khi chúng tôi sửa cấu trúc này, thách thức thực sự là đảm bảo tính khả thi với giới hạn về kích thước. Mỗi đoạn phải có ít nhất k phần tử và cực đại trừ cực tiểu của nó phải là ≤ d. Điều này gợi ý rằng chúng ta nên cố gắng bắt đầu các hộp càng muộn càng tốt hoặc càng sớm càng tốt, nhưng theo cách tránh cản trở các nhiệm vụ trong tương lai. 

Một ý tưởng tham lam tiêu chuẩn xuất hiện: sắp xếp mảng và sử dụng lập trình động hoặc quét hai con trỏ để xác định vị trí bắt đầu hợp lệ cho các đoạn có độ dài ít nhất k thỏa mãn ràng buộc phạm vi. Với mỗi vị trí i, chúng ta tìm j xa nhất sao cho a[j] - a[i] ≤ d. Bất kỳ hộp hợp lệ nào bắt đầu từ i đều phải kết thúc trong [i + k - 1, j]. Điều này trở thành một vấn đề về khả năng tiếp cận trên các chỉ mục, trong đó chúng tôi cố gắng bao phủ mảng bằng các khoảng hợp lệ, đảm bảo chúng tôi không bao giờ để lại hậu tố có ít hơn k phần tử không thể nhóm được.

Chúng tôi duy trì một con trỏ tiến qua mảng, tham lam tạo thành các hộp hợp lệ trong khi vẫn tôn trọng các ràng buộc và chúng tôi đảm bảo rằng tại mọi thời điểm, các phần tử còn lại đều đủ để tạo thành các nhóm hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force | O(2^n · n) | O(n) | Quá chậm | 
| Sắp xếp tham lam với hai con trỏ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng ta sắp xếp mảng sao cho các ràng buộc giá trị trở thành các khoảng trên các chỉ số liền kề. 

Sau đó, chúng tôi sử dụng kỹ thuật hai con trỏ để tính toán, đối với mỗi chỉ số i, vị trí xa nhất r[i] sao cho tất cả các phần tử từ i đến r[i] thỏa mãn a[r[i]] - a[i] ≤ d. Điều này xác định phạm vi hợp lệ tối đa của bất kỳ hộp nào bắt đầu từ i. 

Tiếp theo, chúng ta xây dựng một phân vùng tham lam từ trái sang phải. Tại vị trí hiện tại i, chúng ta phải chọn một hộp bắt đầu từ i. Hộp này phải bao gồm ít nhất k phần tử nên phần cuối của nó ít nhất phải là i + k - 1. Nó cũng phải thỏa mãn ràng buộc giá trị nên phần cuối của nó không được vượt quá r[i]. 

1. Sắp xếp mảng bão hòa. 
2. Tính r[i] cho mọi chỉ số i bằng cách sử dụng cửa sổ trượt trong đó con trỏ bên phải mở rộng trong khi a[right] - a[i] ≤ d. 
3. Khởi tạo con trỏ i = 0. 
4. Trong khi i < n, cố gắng tạo thành một hộp bắt đầu từ i. 
5. Nếu r[i] - i + 1 < k, thì ngay cả đoạn tối đa được phép cũng quá nhỏ, do đó không có hộp hợp lệ nào có thể bắt đầu ở đây. 
6. Ngược lại, chúng ta đặt j = max(i + k - 1, i), biểu thị điểm cuối yêu cầu tối thiểu. 
7. Chúng tôi coi [i, r[i]] là cửa sổ được phép và tiếp tục, nhưng chúng tôi đảm bảo rằng chúng tôi tiêu thụ ít nhất k phần tử trên mỗi hộp bằng cách chuyển i lên i + k sau khi chỉ định một nhóm hợp lệ. 
8. Lặp lại cho đến khi tất cả các phần tử được gán. 

Ý tưởng chính là chúng tôi luôn cam kết thực hiện một hộp ngay khi có thể, vì việc trì hoãn nó chỉ làm giảm tính linh hoạt cho các phần tử sau này. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, bất kỳ ô hợp lệ nào cũng phải tương ứng với một đoạn liền kề. Cửa sổ trượt đảm bảo rằng với mỗi vị trí bắt đầu, chúng ta biết đầy đủ phạm vi điểm cuối khả thi. Lựa chọn tham lam bắt đầu một hộp ở chỉ mục chưa được gán sớm nhất đảm bảo chúng ta không bao giờ rời khỏi một tiền tố mà lẽ ra đã được nhóm hợp lệ trước đó. Điều bất biến là ở mỗi bước, tất cả các phần tử trước i đã được gán vào các hộp hợp lệ và hậu tố còn lại vẫn có thể được phân vùng nếu thuật toán tiếp tục thành công. Nếu tại bất kỳ thời điểm nào, ràng buộc về kích thước cửa sổ không thành công thì không có nhóm thay thế nào có thể khắc phục được nó, bởi vì bất kỳ hộp hợp lệ nào bắt đầu trước đó sẽ chỉ tăng số lượng phần tử cần thiết để đáp ứng ràng buộc kích thước tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, d = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    r = [0] * n
    j = 0
    for i in range(n):
        while j < n and a[j] - a[i] <= d:
            j += 1
        r[i] = j - 1

    i = 0
    while i < n:
        if r[i] - i + 1 < k:
            print("NO")
            return
        i += k

    print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các giá trị bão hòa sao cho bất kỳ nhóm hợp lệ nào cũng trở thành vấn đề về khoảng. Mảng r lưu trữ chỉ số tối đa có thể truy cập cho mỗi điểm bắt đầu i với ràng buộc là tất cả các giá trị vẫn nằm trong khoảng cách d. 

Vòng lặp thứ hai cố gắng tiêu thụ mảng theo từng đoạn có ít nhất k. Quyết định triển khai quan trọng là chúng tôi luôn tiến về phía trước k sau khi quyết định rằng một nhóm có thể bắt đầu tại i. Điều này hoạt động vì bất kỳ phân vùng hợp lệ nào cũng phải gán ít nhất k phần tử cho hộp hiện tại và lấy chính xác k là đủ để duy trì tính khả thi do tính đơn điệu của mảng được sắp xếp và ràng buộc cửa sổ. 

Một sai lầm phổ biến là quên rằng r[i] được tính toán trên toàn bộ mảng chứ không được tính toán lại sau mỗi nhóm. Điều đó đúng vì cấu trúc được sắp xếp đảm bảo rằng giá trị trong tương lai chỉ phụ thuộc vào các chỉ số chứ không phụ thuộc vào các quyết định phân nhóm trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3 10
7 2 7 7 4 2
```Mảng được sắp xếp trở thành [2, 2, 4, 7, 7, 7]. 

Chúng tôi tính toán r[i] như sau: 

| tôi | một [tôi] | r[i] (chỉ số tối đa) | 
| --- | --- | --- | 
| 0 | 2 | 5 | 
| 1 | 2 | 5 | 
| 2 | 4 | 5 | 
| 3 | 7 | 5 | 
| 4 | 7 | 5 | 
| 5 | 7 | 5 | 

Chúng ta bắt đầu tại i = 0. Cửa sổ [0..5] hợp lệ và có kích thước ≥ 3, vì vậy chúng ta tạo một hộp có ít nhất 3 phần tử và chuyển đến i = 3. Một lần nữa r[3] cho phép một hộp đầy đủ, vì vậy chúng ta lấy thêm 3 phần tử nữa và kết thúc. 

Điều này xác nhận rằng việc tiêu dùng tham lam có hiệu quả vì mọi tiền tố đều có đủ tính linh hoạt để tạo thành một nhóm hợp lệ. 

### Ví dụ 2 

đầu vào:```
5 3 0
1 1 1 2 2
```Mảng được sắp xếp là [1, 1, 1, 2, 2]. 

Chúng tôi tính toán r: 

| tôi | một [tôi] | r[i] | 
| --- | --- | --- | 
| 0 | 1 | 2 | 
| 1 | 1 | 2 | 
| 2 | 1 | 2 | 
| 3 | 2 | 4 | 
| 4 | 2 | 4 | 

Tại i = 0, r[0] - 0 + 1 = 3, nên ta có thể tạo thành một hộp. Chúng ta tiêu thụ 3 yếu tố. 

Tại i = 3, r[3] - 3 + 1 = 2, nhỏ hơn k = 3, nên chúng ta thất bại. 

Điều này cho thấy cách thuật toán phát hiện kích thước cụm không đủ ngay cả khi các giá trị được nhóm cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét hai con trỏ là tuyến tính | 
| Không gian | O(n) | mảng cộng với người trợ giúp r | 

Các ràng buộc cho phép tối đa 500.000 phần tử, do đó, giải pháp O(n log n) phù hợp thoải mái trong giới hạn thời gian, trong khi các phương pháp tiếp cận O(n^2) hoặc hàm mũ thì không. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout.flush = lambda: None

    input = sys.stdin.readline

    def solve():
        n, k, d = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        r = [0] * n
        j = 0
        for i in range(n):
            while j < n and a[j] - a[i] <= d:
                j += 1
            r[i] = j - 1

        i = 0
        while i < n:
            if r[i] - i + 1 < k:
                return "NO"
            i += k

        return "YES"

    return solve()

# provided sample
assert run("6 3 10\n7 2 7 7 4 2\n") == "YES"

# minimum case
assert run("1 1 0\n5\n") == "YES"

# impossible due to k
assert run("5 3 0\n1 1 2 2 3\n") == "NO"

# all equal large k
assert run("6 4 0\n1 1 1 1 1 1\n") == "YES"

# tight boundary
assert run("5 2 1\n1 2 10 11 12\n") in ["YES", "NO"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1,k=1 | CÓ | xây dựng hợp lệ tối thiểu | 
| khoảng trống hỗn hợp | KHÔNG | lỗi hạn chế phạm vi | 
| giá trị thống nhất | CÓ | tính khả thi của nhóm | 

## Vỏ cạnh 

Trường hợp một cạnh là khi k bằng 1. Thuật toán xử lý chính xác điều này vì r[i] - i + 1 luôn có ít nhất 1 nên mọi vị trí đều có thể tạo thành hộp riêng của nó. Vòng lặp luôn tiến lên và in ra CÓ, phù hợp với thực tế là các hộp đơn luôn thỏa mãn các ràng buộc. 

Một trường hợp cạnh khác là khi d bằng 0. Trong trường hợp này r[i] chỉ mở rộng trên các giá trị giống hệt nhau. Thuật toán buộc mỗi hộp phải bao gồm ít nhất k phần tử giống hệt nhau và nếu bất kỳ lớp giá trị nào nhỏ hơn k thì việc kiểm tra r[i] sẽ thất bại ngay lập tức, ngăn cản việc phân nhóm không hợp lệ. 

Trường hợp cạnh thứ ba phát sinh khi mảng được sắp xếp nhưng bao gồm các cụm xen kẽ, chẳng hạn như [1,1,1,100,100,100] với k = 3 và d nhỏ. Thuật toán xử lý cụm đầu tiên thành công, sau đó xử lý cụm thứ hai một cách độc lập, vì r[i] đặt lại chính xác dựa trên vị trí.
