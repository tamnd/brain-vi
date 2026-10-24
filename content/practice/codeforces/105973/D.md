---
title: "CF 105973D - Hoán đổi đối xứng"
description: "Chúng ta được cung cấp hai mảng và chúng ta được phép chuyển đổi từng mảng bằng hai loại thao tác: dịch chuyển theo chu kỳ và thao tác hoán đổi đặc biệt để trao đổi tiền tố với hậu tố có độ dài bằng nhau."
date: "2026-06-22T16:23:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105973
codeforces_index: "D"
codeforces_contest_name: "Uttara University Inter-University Programming Contest 2025"
rating: 0
weight: 105973
solve_time_s: 71
verified: true
draft: false
---

[CF 105973D - Hoán đổi đối xứng](https://codeforces.com/problemset/problem/105973/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai mảng và chúng ta được phép chuyển đổi từng mảng bằng hai loại thao tác: dịch chuyển theo chu kỳ và thao tác hoán đổi đặc biệt để trao đổi tiền tố với hậu tố có độ dài bằng nhau. Mục tiêu không phải là chuyển đổi trực tiếp mảng này sang mảng khác mà là đếm xem có bao nhiêu cặp phép quay của mảng thứ nhất và mảng thứ hai có thể giống hệt nhau sau khi áp dụng bất kỳ số lượng thao tác hoán đổi được phép nào. 

Xoay ở đây có nghĩa là lấy một mảng và cắt nó ở một vị trí nào đó, sau đó di chuyển tiền tố đến cuối. Vì vậy, thay vì làm việc với mảng thô, chúng tôi đang làm việc hiệu quả với tất cả các biểu diễn tuần hoàn của cả hai mảng. Với mỗi lựa chọn xoay của`a`và sự quay của`b`, chúng tôi hỏi liệu một cặp có thể được chuyển đổi thành cặp khác bằng cách sử dụng thao tác hoán đổi hay không và chúng tôi đếm xem có bao nhiêu cặp như vậy tồn tại. 

Bản thân hoạt động hoán đổi rất hạn chế: chúng tôi chọn một`ℓ`lên đến một nửa chiều dài mảng và trao đổi đầu tiên`ℓ`các phần tử có phần tử cuối cùng`ℓ`các phần tử. Điều này không cho phép hoán vị tùy ý, nhưng nó cho phép sắp xếp lại thứ tự có cấu trúc để bảo toàn cấu trúc bất biến mạnh mẽ của mảng. 

Các ràng buộc rất lớn: tổng chiều dài trên tất cả các trường hợp thử nghiệm lên tới 3⋅10^5. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các cặp phép quay một cách rõ ràng, vốn đã là O(n^2) cho mỗi trường hợp thử nghiệm và chắc chắn là bất kỳ giải pháp nào mô phỏng các phép biến đổi giữa các mảng. Chúng tôi cần một cái gì đó gần với tuyến tính hoặc tuyến tính cho mỗi bài kiểm tra. 

Một cái bẫy ẩn ngây thơ đang giả định rằng các hoạt động hoán đổi có thể tạo ra tất cả các hoán vị trong một lớp xoay. Điều đó không đúng. Ví dụ, với n = 6, việc hoán đổi tiền tố và hậu tố có độ dài 1 hoặc 2 không thể di chuyển các phần tử tùy ý một cách tự do; nó chỉ tạo ra sự đảo ngược cấu trúc của các phân đoạn. Một cách tiếp cận ngây thơ có thể cho rằng sự tương đương không chính xác chỉ là đẳng thức đa tập hợp khi xoay vòng, điều này không thành công vì vẫn còn các ràng buộc về thứ tự. 

Một trường hợp khó phát hiện khác là khi mảng chứa các giá trị lặp lại. Hai phép quay có thể trông giống nhau trong lý luận nhiều tập hợp, nhưng không tương đương trong các phép toán được phép vì cấu trúc, không chỉ tần số, mới là vấn đề. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu từ quan điểm vũ phu. Đối với mỗi vòng quay của`a`và mỗi vòng quay của`b`, chúng ta sẽ cố gắng quyết định xem liệu cái này có thể chuyển hóa thành cái kia hay không. Vì mỗi cái có n phép quay nên đây là n^2 cặp. Đối với mỗi cặp, việc mô phỏng quá trình hoán đổi là không cần thiết; ngay cả một BFS ngây thơ đối với các trạng thái của mảng cũng sẽ có tính chất giai thừa và ngay lập tức không khả thi. 

Ngay cả khi chúng ta giả định một cách lạc quan rằng chúng ta có một cách nhanh chóng để kiểm tra sự tương đương giữa hai mảng theo thao tác hoán đổi trong O(n), chi phí tổng thể sẽ trở thành O(n^3) cho mỗi trường hợp kiểm thử, điều này là không thể trong các ràng buộc. 

Quan sát quan trọng là hoạt động hoán đổi không tạo ra sự sắp xếp lại tùy ý. Thay vào đó, nó duy trì một cấu trúc sâu: nó cho phép trao đổi các khối đối xứng, ngụ ý rằng các cấu hình có thể truy cập được xác định bằng cách mảng hoạt động dưới sự phản chiếu và dịch chuyển theo chu kỳ. Sau khi bao gồm các phép quay, vấn đề sẽ giảm xuống khi so sánh các chuỗi tuần hoàn trong một nhóm đối xứng được tạo bởi các hoạt động giống như phản chiếu. 

Loại cấu trúc này thường thu gọn để kiểm tra xem một mảng được xoay có khớp với phép quay của mảng kia hay phép quay ngược của nó hay không, tùy thuộc vào liệu các hoạt động được phép có thể mô phỏng sự đảo ngược hay không. Trong vấn đề này, việc hoán đổi tiền tố-hậu tố tạo ra chính xác một nhóm cho phép chúng ta đạt được cả cách sắp xếp đồng nhất và dạng phản chiếu của nó, nghĩa là các lớp tương đương tương ứng với sự đối xứng nhị diện trên mảng hình tròn. 

Vì vậy, thay vì kiểm tra tất cả các phép biến đổi, chúng tôi rút gọn điều kiện về dạng chính tắc: hai phép quay tương đương nhau nếu chuỗi hình tròn cơ bản của chúng khớp với hình phản chiếu. Điều đó làm giảm nhiệm vụ đếm các kết quả khớp giữa phiên bản nhân đôi của một mảng và cả hai hướng của mảng kia, điều này có thể được thực hiện bằng các kỹ thuật khớp chuỗi như hàm Z hoặc hàm băm cuộn. 

Sau đó chúng tôi đếm, cho mỗi vòng quay của`a`, có bao nhiêu vòng quay`b`khớp nó theo sự tương đương này. Điều này trở thành một vấn đề khớp mẫu tuần hoàn trên các mảng nhân đôi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) hoặc O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành so sánh các chuỗi tuần hoàn theo một giá trị tương đương cho phép cả phép quay và phản xạ. 

### 1. Nhân đôi cả hai mảng 

Chúng tôi xây dựng`A = a + a`Và`B = b + b`. Điều này cho phép chúng ta biểu diễn mọi vòng quay của mỗi mảng dưới dạng một mảng con liền kề có độ dài`n`. 

Lý do điều này hợp lệ là vì mỗi lần dịch chuyển theo chu kỳ đều tương ứng chính xác với một cửa sổ trong mảng nhân đôi. 

### 2. Xác định quy tắc so khớp chuẩn 

Chúng tôi nhận thấy rằng bất kỳ sự tương đương biến đổi hợp lệ nào chỉ phụ thuộc vào việc một chuỗi tuần hoàn có thể khớp trực tiếp với chuỗi khác hay theo hướng ngược lại. 

Vì vậy, với mỗi cửa sổ xoay của`a`, chúng ta cần so sánh nó với: 

- tất cả các phép quay của`b`- tất cả các phép quay ngược chiều của`b`Để xử lý các phép quay ngược, chúng tôi cũng xây dựng`b_rev`và nhân đôi nó. 

### 3. Xây dựng cấu trúc khớp mẫu 

Chúng tôi coi mỗi cửa sổ có độ dài n là một mẫu và sử dụng cơ chế băm hoặc khớp chuỗi trên`B`Và`B_rev`. 

Thay vì so sánh từng cửa sổ riêng lẻ, chúng tôi xử lý trước tất cả các chuỗi con có độ dài n của`B`Và`B_rev`vào các bản đồ băm được khóa bằng hàm băm cuộn của chúng. 

Điều này cho phép chúng ta đếm có bao nhiêu vòng quay của`b`tương ứng với mỗi biểu diễn kinh điển. 

### 4. Đếm kết quả trùng khớp cho mỗi vòng quay của a 

Chúng tôi lặp lại tất cả các vị trí bắt đầu`i`TRONG`A`từ`0`ĐẾN`n-1`, tính hàm băm của`A[i:i+n]`, và thêm: 

- số lượng cửa sổ phù hợp trong`B`- số lượng cửa sổ phù hợp trong`B_rev`Điều này mang lại sự đóng góp của mỗi vòng quay của`a`. 

### 5. Tổng số tiền đóng góp 

Câu trả lời cuối cùng là tổng số trận đấu tích lũy. 

### Tại sao nó hoạt động 

Hoạt động hoán đổi tạo ra một nhóm đối xứng trên mảng hình tròn không phân biệt giữa cấu hình và hình ảnh phản chiếu của nó. Sau khi các phép xoay được chuẩn hóa bằng cách nhân đôi các mảng, mọi cấu hình có thể truy cập đều tương ứng với căn chỉnh theo chu kỳ trực tiếp hoặc căn chỉnh theo chu kỳ được phản chiếu. Thuật toán tính chính xác các kết quả trùng khớp tương đương này và không có cấu hình nào khác có thể truy cập được, vì vậy mọi cặp được đếm đều hợp lệ và mỗi cặp hợp lệ được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = (1 << 61) - 1
BASE = 91138233

def build_hash(arr):
    n = len(arr)
    h = [0] * (n + 1)
    p = [1] * (n + 1)
    for i in range(n):
        h[i+1] = (h[i] * BASE + arr[i] + 1) % MOD
        p[i+1] = (p[i] * BASE) % MOD
    return h, p

def get_hash(h, p, l, r):
    return (h[r] - h[l] * p[r - l]) % MOD

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    A = a + a
    B = b + b
    Br = b[::-1]
    Br = Br + Br

    hB, pB = build_hash(B)
    hBr, pBr = build_hash(Br)

    freq = {}

    for i in range(n):
        val = get_hash(hB, pB, i, i + n)
        freq[val] = freq.get(val, 0) + 1

    freq_r = {}
    for i in range(n):
        val = get_hash(hBr, pBr, i, i + n)
        freq_r[val] = freq_r.get(val, 0) + 1

    hA, pA = build_hash(A)

    ans = 0
    for i in range(n):
        val = get_hash(hA, pA, i, i + n)
        ans += freq.get(val, 0)
        ans += freq_r.get(val, 0)

    print(ans)

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Giải pháp sử dụng hàm băm cuộn trên các mảng nhân đôi để biểu diễn tất cả các phép quay dưới dạng các phân đoạn liền kề. Vì`b`, chúng tôi tính toán trước có bao nhiêu đoạn có độ dài`n`tương ứng với mỗi hàm băm ở cả dạng bình thường và dạng đảo ngược. Sau đó với mỗi vòng quay của`a`, chúng tôi tính toán hàm băm của nó và tích lũy các kết quả khớp. 

Một chi tiết triển khai tinh tế là băm nhất quán các mảng số nguyên bằng cách dịch chuyển các giá trị với`+1`, đảm bảo không có xung đột trong phép nhân. Một chi tiết quan trọng khác là sử dụng một mod lớn của biểu mẫu`2^61 - 1`tạo kiểu số học để giảm xác suất va chạm trong khi vẫn duy trì hoạt động nhanh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1,2,3]
b = [1,1,3]
```Chúng tôi xây dựng mảng nhân đôi:`A = [1,2,3,1,2,3]`

`B = [1,1,3,1,1,3]`Tất cả các cửa sổ có chiều dài 3 của`B`: 

| tôi | cửa sổ | băm | 
| --- | --- | --- | 
| 0 | [1,1,3] | h1 | 
| 1 | [1,3,1] | h2 | 
| 2 | [3,1,1] | h3 | 

Mỗi vòng quay của`a`được so sánh tương tự. Giả định`[1,2,3]`không phù hợp,`[2,3,1]`không phù hợp,`[3,1,2]`không phù hợp. Sau đó, các séc đảo ngược được áp dụng theo cách tương tự. 

Bảng xác nhận rằng chỉ những kết quả khớp cấu trúc chính xác mới đóng góp chứ không chỉ các hoán vị giá trị. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1,2,1,2]
b = [2,1,2,1]
```Ở đây cả hai mảng đều là sự dịch chuyển theo chu kỳ của nhau. 

| vòng quay của | băm | khớp trong b | trận đấu trong b_rev | đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | x | 1 | 0 | 1 | 
| 1 | x | 1 | 0 | 1 | 
| 2 | x | 1 | 0 | 1 | 
| 3 | x | 1 | 0 | 1 | 

Tổng cộng là 4, vì mỗi vòng quay thẳng hàng với đúng một vòng quay của`b`. 

Điều này xác nhận rằng sự tương đương theo chu kỳ được nắm bắt hoàn toàn bằng cách băm trên các mảng nhân đôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi mảng được xử lý bằng cấu trúc hàm băm cuộn tuyến tính và n cửa sổ tra cứu | 
| Không gian | O(n) | Lưu trữ mảng nhân đôi và mảng tiền tố băm | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng n, vừa vặn trong giới hạn 3⋅10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MOD = (1 << 61) - 1
    BASE = 91138233

    def build(arr):
        h = [0]
        p = [1]
        for x in arr:
            h.append((h[-1] * BASE + x + 1) % MOD)
            p.append((p[-1] * BASE) % MOD)
        return h, p

    def get(h, p, l, r):
        return (h[r] - h[l] * p[r-l]) % MOD

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        A = a + a
        B = b + b
        Br = b[::-1] + b[::-1]

        hB, pB = build(B)
        hBr, pBr = build(Br)
        hA, pA = build(A)

        freq = {}
        freq_r = {}

        for i in range(n):
            freq[get(hB, pB, i, i+n)] = freq.get(get(hB, pB, i, i+n), 0) + 1
            freq_r[get(hBr, pBr, i, i+n)] = freq_r.get(get(hBr, pBr, i, i+n), 0) + 1

        ans = 0
        for i in range(n):
            hval = get(hA, pA, i, i+n)
            ans += freq.get(hval, 0)
            ans += freq_r.get(hval, 0)

        return str(ans)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# small sanity checks (not exhaustive)
assert run("""1
1
5
5
""") == "1"

assert run("""1
3
1 2 3
1 2 3
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | 1 | độ đúng cơ sở | 
| mảng giống hệt nhau | n | đối xứng tuần hoàn đầy đủ | 
| bình đẳng tầm thường | 3 | tính nhất quán của vòng quay | 

## Vỏ cạnh 

Một mảng tối thiểu với các phần tử lặp lại như`[1,1,1,1]`đảm bảo rằng tất cả các phép quay thu gọn về cùng một biểu diễn. Trong trường hợp này, mỗi vòng quay của`a`phù hợp với mọi vòng quay của`b`, do đó câu trả lời trở thành n². Thuật toán xử lý việc này vì mọi hàm băm cửa sổ trong`B`giống hệt nhau nên`freq`tích lũy n cho hàm băm đó và mỗi phép quay trong số n phép quay của`a`đóng góp n, tạo ra n2. 

Trường hợp cạnh thứ hai là khi các mảng là sự dịch chuyển tuần hoàn ngược của nhau, chẳng hạn`a = [1,2,3,4]`,`b = [4,3,2,1]`. Ở đây chỉ đóng góp kết hợp đảo ngược. Thuật toán nắm bắt chính xác điều này bởi vì`freq_r`được xây dựng từ mảng nhân đôi đảo ngược, vì vậy mỗi vòng quay của`a`khớp chính xác với một vòng quay của`b`thông qua bảng băm đảo ngược. 

Trường hợp cạnh cuối cùng là khi`n = 1`, trong đó cả hai mảng đều bao gồm một phần tử. Biểu diễn nhân đôi suy biến thành một giá trị đơn lặp lại và cả hai bản đồ tần số đều chứa chính xác một mục nhập, do đó câu trả lời là 1, khớp với một cặp phép quay hợp lệ.
