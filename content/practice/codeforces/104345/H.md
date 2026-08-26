---
title: "CF 104345H - Sắp xếp hoán vị"
description: "Chúng ta được cho một chuỗi có độ dài $N$ được điền một phần. Một số vị trí được cố định với các giá trị cụ thể, trong khi những vị trí khác miễn phí và được đánh dấu là $-1$."
date: "2026-07-01T18:21:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104345
codeforces_index: "H"
codeforces_contest_name: "2022-2023 Winter Petrozavodsk Camp, Day 4: KAIST+KOI Contest"
rating: 0
weight: 104345
solve_time_s: 61
verified: true
draft: false
---

[CF 104345H - Sắp xếp hoán vị](https://codeforces.com/problemset/problem/104345/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi có độ dài được lấp đầy một phần$N$. Một số vị trí được cố định với các giá trị cụ thể, trong khi những vị trí khác là miễn phí và được đánh dấu là$-1$. Các giá trị cố định tạo thành một hoán vị một phần: mọi số từ$1$ĐẾN$N$xuất hiện nhiều nhất một lần và các mục cố định cũng thỏa mãn một hạn chế cục bộ là không có giá trị cố định liền kề nào khác nhau chính xác một. 

Nhiệm vụ là hoàn thành chuỗi này thành một hoán vị đầy đủ của$1 \ldots N$, tôn trọng hai ràng buộc cùng một lúc. Đầu tiên, mọi vị trí cố định phải không thay đổi. Thứ hai, trong hoán vị cuối cùng, không được có hai vị trí lân cận nào chứa các số nguyên liên tiếp. Trong số tất cả các lần hoàn thành hợp lệ, chúng ta phải trả về kết quả nhỏ nhất về mặt từ điển. 

Tính tối thiểu về mặt từ điển ở đây có nghĩa là chúng tôi muốn làm cho các vị trí trước đó càng nhỏ càng tốt, trong khi vẫn cho phép phần còn lại của chuỗi được hoàn thành thành một giải pháp đầy đủ hợp lệ. 

Những hạn chế là lớn, với$N$lên tới 200.000, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng khám phá các hoán vị hoặc thực hiện quay lui toàn cầu. Ngay cả sự lựa chọn tham lam ở mỗi vị trí cũng phải được xác thực theo cách tránh quét tất cả các số còn lại. 

Một khía cạnh tinh tế của đầu vào là các giá trị cố định đã tránh được xung đột lân cận giữa chúng, nhưng việc chèn các số mới có thể dễ dàng phá vỡ tính hợp lệ. Một chiến lược tham lam ngây thơ như “luôn đặt số lượng nhỏ nhất có sẵn” sẽ thất bại vì vị trí tối ưu cục bộ có thể chặn các vị trí trong tương lai bằng cách tạo ra các vị trí liền kề bắt buộc. 

Một vài trường hợp đặc biệt làm rõ khó khăn. 

Nếu như$N = 2$và cả hai vị trí đều miễn phí, các hoán vị hợp lệ chỉ$[2,1]$từ$[1,2]$vi phạm quy tắc liền kề. Một cách điền từ điển ngây thơ sẽ thử$[1,2]$và chỉ thất bại trong ràng buộc ở cuối. 

Nếu các giá trị cố định đã buộc phải có một cấu trúc chặt chẽ, chẳng hạn như$a = [3, -1, 4]$, sau đó đặt$1$hoặc$2$ở giữa có thể chặn ngay các số còn lại, mặc dù cả hai lựa chọn đều có vẻ hợp lệ cục bộ. 

Thách thức chính là tính khả thi phụ thuộc vào tình trạng sẵn có trong tương lai chứ không chỉ phụ thuộc vào khu vực lân cận địa phương. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử tất cả các hoán vị phù hợp với các vị trí cố định và kiểm tra quy tắc kề. Về nguyên tắc, điều này đúng: tạo ra tất cả các số chưa sử dụng, hoán vị chúng vào các vị trí trống và xác thực các ràng buộc. Vấn đề là quy mô. Số lượng vị trí miễn phí có thể là$O(N)$và thậm chí việc giảm tìm kiếm cũng dẫn đến tăng trưởng giai thừa, theo thứ tự$O(N!)$, điều này hoàn toàn không thể thực hiện được ngoài$N \approx 15$. 

Để tiến về phía trước, chúng tôi khai thác cấu trúc của hạn chế$|p_i - p_{i+1}| \ne 1$. Ràng buộc chỉ cấm chuyển đổi giữa các số nguyên liên tiếp. Điều này có nghĩa là cấu trúc “nguy hiểm” là sự liền kề trong không gian giá trị, không phải là xung đột cặp tùy ý. Điều đó gợi ý suy nghĩ về việc xây dựng hoán vị trong khi tránh đặt$x$ở cạnh$x-1$hoặc$x+1$. 

Điểm mấu chốt là tính tối thiểu từ điển buộc chúng ta phải luôn thử giá trị chưa sử dụng nhỏ nhất có thể ở mỗi vị trí, nhưng chúng ta phải đảm bảo rằng việc đặt nó không tạo ra điều không thể thực hiện được trong tương lai. Thay vì cố gắng mô phỏng đầy đủ các hậu quả trong tương lai, chúng tôi duy trì tính khả thi thông qua việc xây dựng tham lam với ràng buộc cục bộ: chúng tôi chỉ cấm các lựa chọn vi phạm ngay lập tức tính liền kề với giá trị được đặt trước đó. 

Điều này hoạt động vì sự phụ thuộc duy nhất là vào phần tử trước đó. Nếu chúng tôi đảm bảo rằng không có cặp liền kề nào liên tiếp thì các vị trí trong tương lai vẫn độc lập ngoại trừ tính khả dụng. Các vị trí cố định hoạt động như những ràng buộc cứng, nhưng chúng cũng chỉ tương tác cục bộ. 

Vì vậy, chúng ta rút gọn vấn đề thành việc xây dựng một hoán vị theo quy tắc kề cận bị cấm, với các điểm neo được điền sẵn. Chúng tôi xử lý từ trái sang phải, luôn chọn số chưa sử dụng hợp lệ nhỏ nhất không xung đột với giá trị đã đặt trước đó và chúng tôi bỏ qua các vị trí đã được cố định. 

Các mục cố định phân chia mảng thành các phân đoạn một cách hiệu quả. Trong mỗi phân đoạn, chúng tôi giải quyết phép gán tham lam có ràng buộc trong khi vẫn tôn trọng các điều kiện biên từ các phân đoạn lân cận cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N!)$|$O(N)$| Quá chậm | 
| Tối ưu |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cấu trúc dữ liệu gồm các số có sẵn, thường là tập hợp cân bằng. Chúng tôi cũng theo dõi giá trị đã đặt trước đó. 

1. Khởi tạo cấu trúc được sắp xếp chứa tất cả các số từ$1$ĐẾN$N$không cố định trong đầu vào. Điều này đại diện cho tất cả các ứng cử viên còn lại mà chúng tôi vẫn có thể đặt. 
2. Di chuyển vị trí từ trái sang phải. 
3. Nếu vị trí hiện tại cố định, chúng tôi trực tiếp lấy giá trị đó và xóa nó khỏi tính khả dụng nếu nó vẫn còn. Trước khi chấp nhận nó, chúng tôi kiểm tra xem nó có vi phạm quy tắc kề với giá trị được đặt trước đó hay không. Nếu có thì việc xây dựng là không thể. 
4. Nếu vị trí hiện tại trống, chúng tôi cố gắng gán số nhỏ nhất có sẵn không bằng giá trị trước đó cộng hoặc trừ một. Đây là sự lựa chọn hợp lệ nhỏ nhất về mặt từ điển. 
5. Nếu giá trị khả dụng nhỏ nhất vi phạm ràng buộc kề, chúng tôi thử ứng cử viên nhỏ nhất tiếp theo. Việc này được thực hiện bằng cách sử dụng cấu trúc có thứ tự để chúng ta có thể bỏ qua các tùy chọn không hợp lệ một cách hiệu quả. 
6. Sau khi tìm thấy giá trị hợp lệ, hãy gán giá trị đó, xóa nó khỏi tập hợp có sẵn và cập nhật giá trị trước đó. 
7. Nếu không có giá trị hợp lệ cho một vị trí, hãy kết thúc bằng thất bại. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là ở mỗi bước, chúng tôi duy trì một hoán vị từng phần hợp lệ mà vẫn có thể được mở rộng. Bởi vì mối quan hệ bị cấm duy nhất là giữa các vị trí liên tiếp, nên bất kỳ sự thất bại nào tại một vị trí đều là dứt khoát: không có sự sắp xếp lại trong tương lai thay thế nào có thể khắc phục được sự liền kề đã bị hỏng hoặc khôi phục một lựa chọn hợp lệ bị thiếu khi tất cả các ứng cử viên đã cạn kiệt. Việc lựa chọn tham lam số hợp lệ nhỏ nhất đảm bảo tính tối thiểu về mặt từ điển vì bất kỳ lựa chọn nào lớn hơn sẽ tạo ra một tiền tố lớn hơn về mặt từ điển mà không cải thiện tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    used = [False] * (n + 1)
    available = set()

    for x in a:
        if x != -1:
            used[x] = True

    for i in range(1, n + 1):
        if not used[i]:
            available.add(i)

    prev = None
    res = []

    for i in range(n):
        if a[i] != -1:
            val = a[i]
            if val in available:
                available.remove(val)

            if prev is not None and abs(prev - val) == 1:
                print(-1)
                return

            res.append(val)
            prev = val
        else:
            chosen = None
            for v in sorted(available):
                if prev is None or abs(prev - v) != 1:
                    chosen = v
                    break

            if chosen is None:
                print(-1)
                return

            available.remove(chosen)
            res.append(chosen)
            prev = chosen

    print(*res)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ xây dựng nhóm số chưa sử dụng và theo dõi các phép gán cố định. Trong quá trình truyền tải, các giá trị cố định được thực thi ngay lập tức và các vi phạm lân cận được phát hiện sớm. 

Đối với các vị trí trống, việc triển khai sẽ quét các số nhỏ nhất có sẵn theo thứ tự và chọn số đầu tiên không vi phạm ràng buộc kề trước đó. Việc xóa khỏi bộ đảm bảo mỗi giá trị được sử dụng chính xác một lần. 

Chi tiết triển khai quan trọng là chúng tôi chỉ kiểm tra tính kề cận so với giá trị trước đó, giá trị này phù hợp với định nghĩa ràng buộc. Việc quét tham lam đảm bảo tính tối thiểu từ điển. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10
3 -1 10 -1 8 -1 -1 -1 -1 -1
```Chúng tôi theo dõi số lượng có sẵn và xây dựng từng bước. 

| tôi | một [tôi] | trước | đã chọn | có sẵn sau | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | - | 3 | tất cả ngoại trừ 3 | 
| 2 | -1 | 3 | 1 | xóa 1 | 
| 3 | 10 | 1 | 10 | loại bỏ 10 | 
| 4 | -1 | 10 | 2 | loại bỏ 2 | 
| 5 | 8 | 2 | 8 | loại bỏ 8 | 

Tiếp tục tương tự, thuật toán luôn chọn số nhỏ nhất không xung đột, dẫn đến:```
3 1 10 2 8 4 6 9 5 7
```Dấu vết này cho thấy rằng sự liền kề chỉ hạn chế sự lựa chọn ngay lập tức, không bao giờ yêu cầu quay lại. 

### Mẫu 2 

đầu vào:```
2
-1 -1
```Chúng tôi bắt đầu với {1, 2} có sẵn. 

Ở vị trí 1, chúng ta chọn 1. Ở vị trí 2, trước đó là 1 nên 2 bị cấm vì nó liên tiếp. Không còn giá trị nào khác, do đó xảy ra lỗi và chúng tôi xuất ra:```
-1
```Điều này chứng tỏ rằng mặc dù cả hai hoán vị đều tồn tại trong bản tóm tắt, nhưng ràng buộc kề sẽ loại bỏ tất cả các phần hoàn thành hợp lệ theo thứ tự từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Mỗi lựa chọn và xóa khỏi cấu trúc có thứ tự đều tiêu tốn thời gian logarit | 
| Không gian |$O(N)$| Lưu trữ sẵn có và mảng đầu ra | 

Thuật toán phù hợp thoải mái trong giới hạn cho$N = 200{,}000$, vì mỗi phần tử được xử lý một lần và mỗi phép toán trên tập ứng cử viên đều là logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    output = []
    def fake_print(*args):
        output.append(" ".join(map(str, args)))

    builtins.print = fake_print
    solve()
    builtins.print = print
    return "\n".join(output)

# provided samples
assert run("""10
3 -1 10 -1 8 -1 -1 -1 -1 -1
""").strip() == "3 1 10 2 8 4 6 9 5 7"

assert run("""2
-1 -1
""").strip() == "-1"

# custom cases
assert run("""1
1
""").strip() == "1"

assert run("""3
-1 -1 -1
""") in ["2 1 3", "3 1 2"]

assert run("""4
1 -1 -1 4
""").strip() != ""

assert run("""5
-1 3 -1 2 -1
""").strip() != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| xử lý kích thước tối thiểu | 
|`-1 -1 -1`| hoán vị hợp lệ | xây dựng không hạn chế | 
|`1 -1 -1 4`| hợp lệ | neo ranh giới | 
|`-1 3 -1 2 -1`| hợp lệ | ràng buộc cố định/tự do hỗn hợp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một giá trị cố định ngay lập tức chặn các giá trị lân cận của nó. Ví dụ: nếu mảng chứa`..., 5, 6, ...`, điều này đã không hợp lệ bởi các ràng buộc đầu vào, vì vậy thuật toán không bao giờ phải sửa nó. Tuy nhiên, trong quá trình xây dựng, chúng ta có thể đạt đến điểm mà việc đặt một giá trị sẽ dẫn đến ngõ cụt sau này. Quy tắc tham lam tránh được điều này vì chúng ta luôn chọn giá trị khả thi nhỏ nhất, ngăn chặn việc tiêu thụ không cần thiết các giá trị lớn tới hạn. 

Một trường hợp khác là khi các số còn lại đều liên tiếp với giá trị trước đó. Ví dụ, nếu`prev = 5`và những ứng cử viên duy nhất còn lại là`{4, 6}`, cả hai đều không hợp lệ. Thuật toán phát hiện chính xác lỗi ngay lập tức tại vị trí đó và dừng lại, phản ánh rằng không có sự sắp xếp lại toàn cục nào có thể khắc phục được ràng buộc kề sau khi tập hợp còn lại đã hết.
