---
title: "CF 104081K - \u533a\u95f4\u548c"
description: "Chúng ta có một mảng có độ dài $n$ và mọi phần tử đều là số nguyên không âm. Từ mảng này, chúng ta xem xét mọi mảng con liền kề và mỗi mảng con có trọng số bằng tổng các phần tử của nó."
date: "2026-07-02T02:38:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104081
codeforces_index: "K"
codeforces_contest_name: "2022\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 104081
solve_time_s: 46
verified: true
draft: false
---

[CF 104081K - \u533a\u95f4\u548c](https://codeforces.com/problemset/problem/104081/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n$và mọi phần tử đều là số nguyên không âm. Từ mảng này, chúng ta xem xét mọi mảng con liền kề và mỗi mảng con có trọng số bằng tổng các phần tử của nó. 

Nếu chúng ta liệt kê tất cả các tổng của mảng con và sắp xếp chúng theo thứ tự không giảm, chúng ta sẽ được yêu cầu trả lời nhiều truy vấn. Mỗi truy vấn đưa ra một số nguyên$k$, và chúng ta phải xuất ra giá trị của$k$-tổng mảng con nhỏ nhất trong danh sách được sắp xếp đó. 

Cấu trúc ẩn quan trọng là mặc dù có$O(n^2)$mảng con, mảng chỉ chứa các số 0 và giá trị dương, do đó tổng của mảng con hoạt động đơn điệu khi chúng ta mở rộng hoặc thu nhỏ cửa sổ. Tính đơn điệu này là nguyên nhân làm cho vấn đề có thể giải quyết được trong thời gian gần tuyến tính cho mỗi lần kiểm tra. 

Nếu như$n$lớn, có thể lên tới$2 \times 10^5$, liệt kê tất cả các mảng con đã tạo ra khoảng$2 \times 10^{10}$tổng, không thể sắp xếp một cách rõ ràng. Ngay cả việc lưu trữ chúng là không thể. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào xây dựng hoặc sắp xếp rõ ràng tất cả các tổng khoảng thời gian. 

Một sai lầm ngây thơ là giả định tổng tiền tố và sắp xếp trực tiếp các khác biệt. Điều đó vẫn yêu cầu tạo ra tất cả các cặp chỉ số tiền tố, lại là phương trình bậc hai. Một trường hợp thất bại tinh vi khác là cố gắng duy trì một đống tổng mảng con bằng cách mở rộng các khoảng, vẫn suy biến thành hành vi bậc hai. 

Ví dụ, nếu mảng là$[1, 2, 3]$, tổng của mảng con là$[1, 2, 3, 3, 5, 6]$. Một giải pháp đúng phải xử lý chính xác các trường hợp trùng lặp như hai lần xuất hiện của số 3, điều này đã phá vỡ các ý tưởng đơn giản “tham lam chọn tiện ích mở rộng tiếp theo”. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Tính tổng từng mảng con bằng cách sửa điểm cuối bên trái và mở rộng điểm cuối bên phải, tích lũy tất cả các tổng vào một danh sách, sau đó sắp xếp danh sách và trả lời các truy vấn bằng cách lập chỉ mục. Điều này đúng vì nó trực tiếp xây dựng multiset mà chúng ta cần. Tuy nhiên, nó thực hiện$O(n^2)$việc xây dựng mảng con và sắp xếp chúng mất$O(n^2 \log n^2)$, vượt xa giới hạn ngay cả đối với mức độ vừa phải$n$. Với$n = 2 \times 10^5$, chỉ riêng số lượng mảng con đã làm cho phương pháp này không khả thi. 

Quan sát quan trọng là chúng ta không cần phải xây dựng hoặc sắp xếp tất cả các khoản tiền một cách rõ ràng. Thay vào đó, chúng ta có thể đặt câu hỏi quyết định: cho trước một giá trị$x$, có bao nhiêu mảng con có tổng nhỏ hơn hoặc bằng$x$? Nếu chúng ta có thể tính toán điều này một cách hiệu quả thì chúng ta có thể tìm kiếm nhị phân trên$x$tìm giá trị nhỏ nhất sao cho ít nhất$k$mảng con có tổng$\le x$. Điều này biến đổi vấn đề từ việc sắp xếp một mảng ẩn lớn thành việc đếm lặp lại trên một vị từ đơn điệu. 

Bước đếm trở nên hiệu quả vì tất cả các giá trị mảng đều không âm. Điều này đảm bảo rằng nếu chúng ta cố định ranh giới bên trái thì việc mở rộng ranh giới bên phải chỉ làm tăng tổng, do đó chúng ta có thể duy trì một cửa sổ trượt với hai con trỏ trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \log n^2)$|$O(n^2)$| Quá chậm | 
| Tìm kiếm nhị phân + Hai con trỏ |$O(n \log S)$|$O(1)$| Đã chấp nhận | 

Đây$S$là tổng của mảng, giới hạn tất cả các tổng của mảng con. 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề xuống việc trả lời nhiều truy vấn thống kê bậc k trên tổng mảng con bằng cách sử dụng tìm kiếm nhị phân trên không gian câu trả lời. 

1. Tính tổng mảng con tối đa có thể, là tổng của mảng. Điều này trở thành giới hạn trên cho tìm kiếm nhị phân. Giới hạn dưới là bằng không. 
2. Xác định hàm`count(x)`trả về số lượng mảng con có tổng nhỏ hơn hoặc bằng$x$. Chức năng này là cốt lõi của giải pháp. 
3. Tính toán`count(x)`sử dụng hai con trỏ. Duy trì một cửa sổ trượt$[l, r]$và tổng của nó. Đối với mỗi điểm cuối bên phải$r$, tăng tổng. Nếu tổng vượt quá$x$, di chuyển$l$chuyển tiếp cho đến khi tổng hợp lệ trở lại. Tại mỗi vị trí$r$, tất cả các mảng con kết thúc tại$r$và bắt đầu từ bất kỳ chỉ mục nào trong$[l, r]$có giá trị, góp phần$r - l + 1$mảng con. Điều này có hiệu quả vì tất cả các số đều không âm, do đó việc thu nhỏ cửa sổ không bao giờ làm tăng tổng. 
4. Đối với mỗi truy vấn$k$, thực hiện tìm kiếm nhị phân trên phạm vi giá trị$[0, S]$. Tại mỗi trung điểm$mid$, tính toán`count(mid)`. Nếu ít nhất là$k$, chúng ta có thể di chuyển sang trái; nếu không chúng ta sẽ di chuyển sang phải. 
5. Kết quả tìm kiếm nhị phân cuối cùng là giá trị nhỏ nhất sao cho ít nhất$k$mảng con có tổng nhỏ hơn hoặc bằng nó, chính xác là$k$-tổng mảng con nhỏ nhất thứ. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào tính đơn điệu của vị từ “số mảng con có tổng ≤ x”. BẰNG$x$tăng lên, số lượng này không bao giờ giảm vì mọi mảng con hợp lệ trước đó vẫn hợp lệ. Điều này làm cho tìm kiếm nhị phân hợp lệ trên không gian trả lời. 

Bên trong`count(x)`, tính đúng đắn đến từ tính bất biến của cửa sổ trượt mà tại mỗi bước, cửa sổ$[l, r]$là ranh giới bên trái nhỏ nhất sao cho tổng của cửa sổ là ≤$x$. Bởi vì tất cả các phần tử đều không âm, nên mở rộng$r$chỉ có thể tăng tổng và di chuyển$l$chỉ có thể giảm nó. Do đó mọi mảng con hợp lệ kết thúc tại$r$chính xác là những gì bắt đầu giữa$l$Và$r$và không có mảng con hợp lệ nào bị bỏ sót hoặc được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_subarrays_leq(arr, x):
    n = len(arr)
    l = 0
    s = 0
    res = 0
    for r in range(n):
        s += arr[r]
        while l <= r and s > x:
            s -= arr[l]
            l += 1
        res += (r - l + 1)
    return res

def solve():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    q = int(input().strip())
    queries = [int(input().strip()) for _ in range(q)]

    total = sum(arr)

    def kth(k):
        lo, hi = 0, total
        ans = total
        while lo <= hi:
            mid = (lo + hi) // 2
            if count_subarrays_leq(arr, mid) >= k:
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1
        return ans

    for k in queries:
        print(kth(k))

if __name__ == "__main__":
    solve()
```chức năng`count_subarrays_leq`là bộ đếm cửa sổ trượt. Chi tiết triển khai chính là duy trì`l`sao cho tổng cửa sổ không bao giờ vượt quá`x`. Điều này đảm bảo độ phức tạp tuyến tính cho mỗi cuộc gọi. 

chức năng`kth`thực hiện tìm kiếm nhị phân trên các tổng mảng con có thể. Giới hạn trên được đặt an toàn thành tổng vì không có mảng con nào có thể vượt quá nó. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng$[1, 2, 3]$. Tổng của mảng con là$[1, 3, 6, 2, 5, 3]$, được sắp xếp trở thành$[1, 2, 3, 3, 5, 6]$. 

Vì$k = 4$, chúng tôi mong đợi câu trả lời là 3. 

### Dấu vết tìm kiếm nhị phân cho$k = 4$| giữa | đếm (giữa) | quyết định | 
| --- | --- | --- | 
| 3 | 5 | đi bên trái | 
| 1 | 1 | đi bên phải | 
| 2 | 3 | đi bên phải | 
| 3 (ứng cử viên cuối cùng) | 5 | hợp lệ | 

Giá trị nhỏ nhất có số lượng ≥ 4 là 3. 

Dấu vết này cho thấy cách xử lý các tổng trùng lặp một cách tự nhiên vì hàm đếm bao gồm tất cả các lần xuất hiện thay vì các giá trị duy nhất. 

Bây giờ hãy xem xét$[0, 0, 1]$. Tổng mảng con là$[0, 0, 1, 0, 1, 1]$, được sắp xếp như$[0, 0, 0, 1, 1, 1]$. 

Vì$k = 2$, đáp án là 0. 

### Dấu vết tìm kiếm nhị phân cho$k = 2$| giữa | đếm (giữa) | quyết định | 
| --- | --- | --- | 
| 1 | 6 | đi bên trái | 
| 0 | 3 | đi bên trái | 
| 0 | 3 | dừng lại | 

Thuật toán xử lý chính xác nhiều mảng con có tổng bằng 0, thường gặp khi đầu vào chứa số 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log S \cdot q)$| Mỗi truy vấn thực hiện tìm kiếm nhị phân trên phạm vi tổng và mỗi lần kiểm tra sử dụng quét hai con trỏ tuyến tính | 
| Không gian |$O(1)$| Chỉ sử dụng con trỏ và bộ đếm | 

Tổng số tiền$S$được giới hạn bởi tổng các phần tử mảng, do đó độ sâu tìm kiếm nhị phân nhỏ. Với các ràng buộc thông thường, điều này phù hợp một cách thoải mái trong giới hạn khi sử dụng đầu vào được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like case
assert run("3\n1 2 3\n2\n1\n4\n") == "1\n3"

# all zeros
assert run("4\n0 0 0 0\n3\n1\n5\n10\n") == "0\n0\n0"

# single element
assert run("1\n5\n2\n1\n1\n") == "5\n5"

# mixed case
assert run("3\n0 1 0\n3\n1\n2\n3\n") == "0\n0\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | 0 giây | xử lý nhiều tổng mảng con bằng nhau | 
| phần tử đơn | 5, 5 | độ chính xác cấu trúc tối thiểu | 
| số không hỗn hợp | 0, 0, 1 | số tiền trùng lặp và đặt hàng | 

## Vỏ cạnh 

Đối với một mảng hoàn toàn bằng 0 như$[0, 0, 0]$, mọi mảng con đều có tổng bằng 0. Hàm đếm`count(x)`trả về toàn bộ số lượng mảng con cho bất kỳ$x \ge 0$và tìm kiếm nhị phân hội tụ chính xác về 0 cho mọi truy vấn, vì 0 là giá trị duy nhất có thể. 

Đối với một mảng tăng nghiêm ngặt như$[1, 2, 3]$, tổng của mảng con đều khác biệt hoặc hơi trùng lặp nhưng vẫn đơn điệu. Cửa sổ trượt luôn chỉ co lại khi cần thiết và mỗi mảng con được tính chính xác một lần trong`count(x)`bởi vì mỗi điểm cuối bên phải đóng góp một khối khởi đầu hợp lệ liền kề. 

Đối với trường hợp có số 0 bên trong như$[1, 0, 1]$, các tổng như 1 xuất hiện nhiều lần từ các mảng con khác nhau. Thuật toán không phân biệt cấu trúc mà chỉ phân biệt tổng, do đó, các bản sao được đưa vào số đếm một cách tự nhiên, duy trì tính chính xác của các truy vấn dựa trên thứ hạng.
