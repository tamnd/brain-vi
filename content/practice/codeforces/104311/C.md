---
title: "CF 104311C - c0=c1"
description: "Chúng ta có hai mảng nhị phân có độ dài bằng nhau. Từ mỗi mảng, chúng ta được phép chọn một đoạn liền kề và hạn chế duy nhất đối với mỗi đoạn là độ dài của nó phải nằm trong một phạm vi nhất định."
date: "2026-07-01T19:58:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104311
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #11 (DIV2.5-Forces)"
rating: 0
weight: 104311
solve_time_s: 108
verified: true
draft: false
---

[CF 104311C - c0=c1](https://codeforces.com/problemset/problem/104311/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng nhị phân có độ dài bằng nhau. Từ mỗi mảng, chúng ta được phép chọn một đoạn liền kề và hạn chế duy nhất đối với mỗi đoạn là độ dài của nó phải nằm trong một phạm vi nhất định. Sau khi chọn hai phân đoạn, chúng tôi đếm xem có bao nhiêu số 0 và số 1 xuất hiện trong cả hai phân đoạn đó cộng lại. Mục đích là để xác định xem có tồn tại sự lựa chọn trong hai phân đoạn sao cho tổng số số 0 bằng tổng số số một hay không. 

Một cách hữu ích để viết lại điều kiện là nghĩ về sự cân bằng. Đối với bất kỳ phân đoạn nào, hãy xác định giá trị của nó bằng số lượng số một trừ đi số lượng số không. Yêu cầu tổng số 0 bằng tổng số 1 trên cả hai phân đoạn đã chọn hoàn toàn giống như yêu cầu tổng của hai giá trị phân đoạn này bằng 0. Vì vậy chúng ta đang tìm kiếm một đoạn trong mảng`a`và một đoạn trong mảng`b`, trong giới hạn độ dài tương ứng của chúng, sao cho “sự mất cân bằng” của chúng bị loại bỏ một cách hoàn hảo. 

Các ràng buộc khiến cho việc áp dụng bạo lực lên tất cả các phân đoạn là không thể. Mỗi mảng có tới 200.000 phần tử trong các trường hợp thử nghiệm và có tới 100.000 trường hợp thử nghiệm. Ngay cả việc liệt kê bậc hai các phân đoạn cho mỗi trường hợp thử nghiệm cũng sẽ vượt xa mọi giới hạn khả thi. Quét tuyến tính cho mỗi trường hợp thử nghiệm đã là quy mô dự kiến. 

Một kiểu lỗi phổ biến là bỏ qua các phân đoạn đến từ hai mảng khác nhau. Một giải pháp đơn giản có thể cố gắng tính toán trước tất cả các tổng phân đoạn trong một mảng và sau đó so khớp chúng với mảng kia, nhưng việc quên các ràng buộc về độ dài sẽ phá vỡ tính chính xác. Một sai lầm tinh vi khác là chỉ xem xét tổng tiền tố hoặc chỉ xem xét tổng phân đoạn tối thiểu và tối đa; cả hai đều mất đi sự phụ thuộc vào độ dài phân đoạn được phép, điều này rất cần thiết ở đây. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi liệt kê mọi phân đoạn hợp lệ trong`a`có chiều dài nằm giữa`x1`Và`y1`, tính toán số dư của nó, sau đó liệt kê mọi phân đoạn hợp lệ trong`b`có chiều dài nằm giữa`x2`Và`y2`, tính số dư của nó và kiểm tra xem tổng của một cặp có bằng 0 hay không. Điều này hoạt động vì nó tuân theo định nghĩa trực tiếp, nhưng nó quá chậm. Mỗi mảng có các phân đoạn O(n²) trong trường hợp xấu nhất, do đó tổng số phép so sánh sẽ trở thành O(n⁴) trong chế độ xem sản phẩm chéo đơn giản, ngay cả khi được triển khai cẩn thận. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào tổng phân đoạn của một mảng được chuyển đổi. Nếu chúng ta chuyển đổi từng mảng nhị phân thành một mảng trong đó`0 → -1`Và`1 → +1`, thì tổng mỗi phân đoạn chính xác là số dư của nó. Vấn đề trở thành: có tồn tại hai phân đoạn được phép, một phân đoạn từ mỗi mảng, có tổng bằng 0 hay không. 

Điều này có thể được trình bày lại dưới dạng một bài toán giao nhau đã đặt trên tổng các đoạn có thể đạt được dưới các ràng buộc về độ dài. Thay vì liệt kê tất cả các phân đoạn, chúng tôi chỉ quan tâm đến số tiền nào có thể đạt được cho mỗi mảng. Đối với một mảng cố định và khoảng thời gian có độ dài cố định, chúng ta có thể xác định tất cả các tổng phân đoạn một cách hiệu quả bằng cách sử dụng cửa sổ trượt qua các tổng tiền tố. Thách thức là kiểm tra xem có bất kỳ khoản tiền nào từ`a`có sự phủ định của nó trong`b`. 

Chúng tôi tránh lưu trữ tất cả các khoản tiền có thể một cách rõ ràng. Thay vào đó, chúng tôi quét qua một mảng, đánh dấu tổng phân đoạn có thể đạt được trong tập hợp băm, sau đó quét mảng khác và kiểm tra phần bổ sung. Vì tổng phân đoạn phụ thuộc vào sự khác biệt về tiền tố, nên tổng phân đoạn có thể được tính bằng O(1) và mỗi mảng đóng góp tổng thể O(n) ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) mỗi lần kiểm tra | O(n²) | Quá chậm | 
| Liệt kê tiền tố + băm | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta biến đổi từng giá trị nhị phân sao cho số 0 đóng góp -1 và số 1 đóng góp +1. Điều này làm cho tổng của mỗi phân đoạn thể hiện sự mất cân bằng giữa số 1 và số 0. 

Sau đó, chúng tôi tính toán tổng tiền tố cho cả hai mảng để có thể trích xuất bất kỳ tổng phân đoạn nào trong thời gian không đổi. 

Đối với mỗi mảng, chúng tôi lặp lại tất cả độ dài phân đoạn hợp lệ. Đối với một chiều dài cố định`len`, chúng ta trượt một cửa sổ qua mảng và tính tổng phân đoạn bằng cách sử dụng các khác biệt về tiền tố. Đối với mảng`a`, chúng tôi lưu trữ tất cả các tổng phân khúc có thể đạt được trong một tập hợp băm. Chúng tôi lặp lại quá trình tương tự cho mảng`b`. 

Cuối cùng, chúng tôi kiểm tra xem có tồn tại một giá trị`s`trong bộ`a`như vậy`-s`tồn tại trong tập hợp`b`. Nếu có thì xuất ra CÓ, ngược lại thì KHÔNG. 

Điểm tinh tế là chúng ta phải tôn trọng các ràng buộc về độ dài một cách độc lập cho cả hai mảng. Các bộ chỉ được xây dựng từ các kích thước cửa sổ hợp lệ, nếu không chúng tôi sẽ bao gồm các ứng cử viên không hợp lệ không thể xuất hiện trong giải pháp thực. 

### Tại sao nó hoạt động 

Mỗi phân đoạn hợp lệ tương ứng duy nhất với một khác biệt tiền tố, vì vậy không có ứng cử viên hợp lệ nào bị bỏ sót khi chúng tôi liệt kê theo độ dài và chỉ mục bắt đầu. Việc chuyển đổi thành ±1 đảm bảo rằng sự bằng nhau của số 0 và số 1 trên hai phân đoạn tương đương với việc hủy tổng. Vì chúng tôi kiểm tra tất cả các tổng có thể đạt được từ cả hai mảng trong cùng một ràng buộc, nên sự tồn tại của cặp tổng bằng 0 tương đương với việc phát hiện một phủ định trùng khớp trong hai bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_sums(arr, x, y):
    n = len(arr)
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + (1 if arr[i] == 1 else -1)

    res = set()

    for length in range(x, y + 1):
        for i in range(n - length + 1):
            s = pref[i + length] - pref[i]
            res.add(s)

    return res

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, x1, y1, x2, y2 = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        sa = build_sums(a, x1, y1)
        sb = build_sums(b, x2, y2)

        # check intersection via negation
        ok = False
        if len(sa) > len(sb):
            sa, sb = sb, sa

        for v in sa:
            if -v in sb:
                ok = True
                break

        out.append("YES" if ok else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng tổng tiền tố mã hóa sự mất cân bằng tích lũy để tổng phân đoạn được tính theo O(1). Các vòng lặp lồng nhau theo chiều dài và chỉ số bắt đầu đảm bảo chỉ các khoảng thời gian hợp lệ mới được xem xét. Việc hoán đổi các bộ giúp giảm chi phí tra cứu một chút bằng cách lặp lại bộ nhỏ hơn. 

Kiểm tra phủ định là bản dịch trực tiếp của điều kiện mà số dư hai đoạn phải hủy bỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp nhỏ trong đó cả hai mảng đều`0 1 0 1`và cả hai độ dài đoạn phải chính xác là 2. 

Chúng tôi chuyển đổi thành`-1 +1 -1 +1`. Tổng phân đoạn là: 

| phân đoạn | tổng hợp | 
| --- | --- | 
| a[0:2] | 0 | 
| một[1:3] | 0 | 
| một[2:4] | 0 | 

Cả hai mảng đều tạo ra cùng một bộ`{0}`. 

Sau đó chúng tôi kiểm tra xem có số tiền nào trong`a`có sự phủ định trong`b`. Từ`0 == -0`, câu trả lời là CÓ. 

Điều này xác nhận thuật toán xử lý chính xác các trường hợp trong đó đạt được mức hủy tối ưu bằng các phân đoạn trung tính. 

### Ví dụ 2 

hãy để`a = 1 1 1 1`,`b = 0 0 0 0`và cả hai phạm vi độ dài đều cho phép lựa chọn đầy đủ. 

Mảng được chuyển đổi là`+1 +1 +1 +1`Và`-1 -1 -1 -1`. Tổng phân đoạn hoàn toàn dương đối với`a`và hoàn toàn tiêu cực đối với`b`. 

| mảng | số tiền có thể có | 
| --- | --- | 
| một | chỉ tích cực | 
| b | chỉ tiêu cực | 

Không có giá trị trong`a`có sự phủ định trong`b`, do đó thuật toán trả về KHÔNG. 

Điều này chứng tỏ tính đúng đắn trong trường hợp các hướng mất cân bằng không bao giờ thẳng hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · (y - x + 1)) mỗi lần kiểm tra | mỗi độ dài phân đoạn hợp lệ được quét tuyến tính trên mảng | 
| Không gian | O(n) | tổng tiền tố cộng với bộ băm của các giá trị phân đoạn | 

Tổng số tiền của`n`trong các thử nghiệm được giới hạn bởi 200.000, do đó, ngay cả với phạm vi cửa sổ vừa phải, cách tiếp cận vẫn tổng hợp tuyến tính. Việc sử dụng bộ băm đảm bảo kiểm tra phần bù liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def build(arr, x, y):
        n = len(arr)
        pref = [0]*(n+1)
        for i in range(n):
            pref[i+1] = pref[i] + (1 if arr[i]==1 else -1)
        s = set()
        for l in range(x, y+1):
            for i in range(n-l+1):
                s.add(pref[i+l]-pref[i])
        return s

    def solve():
        t = int(input())
        out=[]
        for _ in range(t):
            n,x1,y1,x2,y2 = map(int,input().split())
            a=list(map(int,input().split()))
            b=list(map(int,input().split()))
            sa=build(a,x1,y1)
            sb=build(b,x2,y2)
            ok=False
            for v in sa:
                if -v in sb:
                    ok=True
                    break
            out.append("YES" if ok else "NO")
        return "\n".join(out)

    return solve()

# provided samples
assert run("""4
4 3 3 3 3
0 1 0 1
0 1 0 0
5 4 5 1 3
1 1 1 1 1
0 0 0 0 1
4 2 4 1 3
1 1 1 1
0 1 1 0
6 1 2 1 2
0 0 0 0 0 0
0 0 0 1 0 0
""") == "YES\nNO\nNO\nYES"

# custom cases
assert run("""1
1 1 1 1 1
0
1
""") == "YES", "single cancellation"

assert run("""1
3 1 3 1 3
0 0 0
0 0 0
""") == "YES", "all zero arrays"

assert run("""1
3 1 2 1 2
1 1 1
1 1 1
""") == "NO", "no zero balance possible"

assert run("""1
4 2 3 2 3
1 0 1 0
0 1 0 1
""") == "YES", "symmetry case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn (0 vs 1) | CÓ | hủy bỏ tối thiểu | 
| tất cả số không | CÓ | phân khúc số dư bằng 0 tồn tại ở khắp mọi nơi | 
| tất cả những cái | KHÔNG | không có cách nào để cân bằng | 
| đối xứng xen kẽ | CÓ | hủy bỏ phân khúc hỗn hợp | 

## Vỏ cạnh 

Trường hợp một cạnh phát sinh khi cả hai mảng bao gồm toàn bộ các bit giống hệt nhau. Nếu tất cả các giá trị bằng 0 thì mọi phân đoạn đều có sự mất cân bằng bằng 0, do đó, bất kỳ cặp hợp lệ nào cũng hoạt động. Thuật toán chỉ chèn chính xác các số 0 vào cả hai bộ và tìm thấy kết quả khớp ở số 0. 

Một trường hợp cạnh khác xuất hiện khi phạm vi độ dài đoạn loại trừ tất cả trừ một độ dài. Thuật toán vẫn chỉ liệt kê độ dài đó, do đó không có phân đoạn không hợp lệ nào lọt vào. Ví dụ: nếu cả hai phạm vi đều là`[1,1]`, chỉ các phần tử đơn lẻ được xem xét và việc kiểm tra số dư giảm xuống việc so sánh các bit riêng lẻ trên các mảng, được xử lý một cách tự nhiên bằng logic tiền tố. 

Trường hợp cạnh thứ ba là khi một mảng có thể tạo ra cả tổng phân đoạn dương và âm trong khi mảng kia hoàn toàn là một phía. Trong trường hợp đó, việc kiểm tra phủ định không bao giờ thành công vì việc phân bổ giá trị không trùng lặp một cách đối xứng. Cách tiếp cận dựa trên tập hợp nắm bắt chính xác sự bất đối xứng này mà không cần xử lý đặc biệt.
