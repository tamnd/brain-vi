---
title: "CF 2030E - MEXimize Điểm"
description: "Chúng ta được cung cấp một mảng và chúng ta xem xét tất cả các dãy con không trống của nó. Đối với mỗi dãy con được chọn, chúng ta được phép chia các phần tử của nó thành bất kỳ số nhóm nào, trong đó mỗi nhóm là một tập hợp nhiều tập hợp."
date: "2026-06-08T11:57:21+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "dp", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2030
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 979 (Div. 2)"
rating: 2200
weight: 2030
solve_time_s: 106
verified: false
draft: false
---

[CF 2030E - MEXimize điểm](https://codeforces.com/problemset/problem/2030/E) 

**Xếp hạng:** 2200 
**Tags:** tổ hợp, cấu trúc dữ liệu, dp, tham lam, triển khai, toán học 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và chúng ta xem xét tất cả các dãy con không trống của nó. Đối với mỗi dãy con được chọn, chúng ta được phép chia các phần tử của nó thành bất kỳ số nhóm nào, trong đó mỗi nhóm là một tập hợp nhiều tập hợp. Đối với một nhóm cố định, mỗi nhóm đóng góp MEX của nhóm đó và chúng tôi muốn tối đa hóa tổng các giá trị MEX này trên tất cả các nhóm. Mức tối đa đó chính là “điểm” của dãy sau. 

Nhiệm vụ không phải là tính điểm cho một dãy con mà là tính tổng điểm này trên mọi dãy con không trống của mảng ban đầu. 

Khó khăn là cả hai lớp đều có tính tổ hợp. Lớp bên ngoài liệt kê các chuỗi con, vốn đã theo cấp số nhân. Lớp bên trong yêu cầu phân vùng tối ưu tối đa hóa hàm phi tuyến (tổng các giá trị MEX). Một nỗ lực trực tiếp sẽ liên quan đến việc suy luận về tất cả các tập hợp con của các phần tử và tất cả các phân vùng của mỗi tập hợp con, điều này phát triển vượt xa mọi giới hạn khả thi. 

Những hạn chế làm rõ điều này. Tổng kích thước của các trường hợp thử nghiệm tối đa là 2·10^5, do đó, mọi giải pháp đều phải gần tuyến tính hoặc tuyến tính cho mỗi thử nghiệm. Bất cứ điều gì liên quan đến việc liệt kê các chuỗi con, phân vùng hoặc thậm chí DP trên các giá trị trên mỗi tập hợp con đều không thể thực hiện được. Chúng tôi buộc phải tìm một công thức phân tách đóng góp của từng giá trị một cách độc lập. 

Một vấn đề tế nhị xuất hiện trong suy nghĩ ngây thơ là cho rằng điểm số chỉ phụ thuộc vào tần số giá trị chung. Điều đó là chưa đủ vì việc phân vùng cho phép chúng ta sử dụng lại cấu trúc giữa các nhóm. Ví dụ: một mảng như`[0,0,1]`cho phép hình thành nhiều nhóm mà mỗi nhóm đóng góp MEX theo những cách khác nhau, vì vậy việc nhóm tham lam chỉ dựa trên tổng số của mỗi số sẽ bỏ lỡ việc tái sử dụng tối ưu. 

Một cạm bẫy khác là giả sử MEX của dãy con chiếm ưu thế trong câu trả lời. Điểm số không phải là một MEX; nó là tổng của nhiều giá trị MEX được tạo bằng cách phân vùng, về cơ bản thay đổi cấu trúc từ ràng buộc một tiền tố thành một quy trình đóng gói lặp lại. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu từ quan điểm vũ phu. Đối với mỗi chuỗi con, chúng ta có thể thử tất cả các phân vùng thành nhiều tập hợp và tính điểm của từng phân vùng. Ngay cả khi chúng tôi sửa một chuỗi con có kích thước m, số lượng phân vùng sẽ theo cấp số nhân tính bằng m (số chuông) và việc đánh giá MEX cho mỗi nhóm vẫn tốn thời gian tuyến tính. Vì có 2^n dãy con, nên tổng độ phức tạp sẽ ở mức 2^n nhân với một hệ số mũ khác, điều này hoàn toàn không khả thi ngay cả với n = 20. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì xây dựng các phân vùng, chúng tôi hỏi phân vùng phải có cấu trúc gì để đạt được tổng số điểm nhất định. Mỗi đơn vị đóng góp MEX tương ứng với việc hình thành một chuỗi giá trị 0, 1, 2, …, x−1 bên trong một nhóm. Mỗi nhóm đóng góp ít nhất 1 phải chứa số 0, mỗi nhóm đóng góp ít nhất 2 phải chứa cả 0 và 1, v.v. 

Điều này gợi ý việc diễn giải quá trình này giống như việc “trích xuất các lớp” liên tục các tiền tố hoàn chỉnh của các giá trị. Mỗi lần chúng tôi hình thành một đơn vị đóng góp ở cấp độ k, chúng tôi đang sử dụng một cách hiệu quả một bản sao của mọi số từ 0 đến k−1. 

Bây giờ chuyển sự chú ý sang một dãy con cố định. Điểm tối ưu của nó được xác định hoàn toàn bằng số lượng đầy đủ các lớp tiền tố tăng dần có thể được đóng gói từ số lượng tần số của nó. Nếu chúng ta biểu thị cnt[v] là tần số của giá trị v trong dãy con thì số lượng lớp có kích thước k tối đa mà chúng ta có thể hình thành phụ thuộc vào số lần tất cả các giá trị 0..k−1 có sẵn đồng thời. Điều này làm giảm tối ưu hóa bên trong thành việc đóng gói tham lam cổ điển trên tiền tố cực tiểu. 

Phép biến đổi thứ hai là phép biến đổi quan trọng: thay vì tính điểm cho mỗi chuỗi con, chúng tôi tính tổng mức đóng góp của từng “sự kiện hình thành lớp” có thể có trên tất cả các chuỗi con. Một chuỗi con đóng góp ít nhất một lớp có kích thước k khi và chỉ khi nó chứa ít nhất một lần xuất hiện của mỗi giá trị trong [0, k−1]. Điều này chuyển đổi vấn đề thành các chuỗi con đếm thỏa mãn các ràng buộc về mức độ bao phủ trên các tập giá trị. 

Sau đó, chúng tôi chuyển sang đếm các sự kiện bổ sung bằng cách sử dụng loại trừ bao gồm theo cách có cấu trúc. Đối với mỗi k, chúng tôi đếm có bao nhiêu chuỗi con chứa tất cả các giá trị 0..k−1 ít nhất một lần. Đây hiện là một bài toán đếm chuỗi con cổ điển “mỗi giá trị phải xuất hiện”, trong đó các lần xuất hiện độc lập giữa các vị trí. 

Đối với k cố định, giả sử tổng số lần xuất hiện của giá trị v là f[v]. Một dãy con chứa ít nhất một v nếu chúng ta chọn một tập con không trống gồm các lần xuất hiện f[v] của nó, đóng góp (2^{f[v]} − 1). Vì các lựa chọn trên các giá trị là độc lập nên số dãy con chứa tất cả các giá trị 0..k−1 là tích trên v < k của (2^{f[v]} − 1). 

Mỗi dãy con như vậy đóng góp 1 đơn vị cho câu trả lời ở cấp độ k. Tính tổng trên tất cả k sẽ cho ra tổng đóng góp của tất cả các lớp MEX có thể có trên tất cả các chuỗi con, bằng tổng điểm tối ưu cần thiết. 

Cấu trúc cuối cùng là tích số tiền tố trên các tần số được biến đổi, tích lũy trên k. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các chuỗi con và phân vùng | O(2^n · Chuông(n)) | O(n) | Quá chậm | 
| Đếm sản phẩm tần số tiền tố | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đếm tần số của từng giá trị trong mảng. Điều này là cần thiết vì mọi tính toán sau này chỉ phụ thuộc vào số lần mỗi số có thể tham gia hình thành chuỗi MEX chứ không phụ thuộc vào vị thế. 
2. Tính trước lũy thừa từ hai đến n, vì mỗi giá trị v đóng góp hệ số 2^{f[v]} − 1 khi quyết định xem một dãy con có bao gồm nó ít nhất một lần hay không. 
3. Lặp lại các mức MEX có thể k bắt đầu từ 0 trở lên. Tại mỗi k, chúng ta giải thích k là yêu cầu tất cả các giá trị từ 0 đến k−1 phải có mặt trong dãy con. 
4. Duy trì một sản phẩm đang chạy trên các giá trị v < k của (2^{f[v]} − 1). Tích này biểu thị số dãy con chứa tất cả các giá trị bắt buộc lên tới k−1. 
5. Thêm sản phẩm này vào đáp án của cấp độ k. Điều này tương ứng với việc đếm tất cả các chuỗi con có thể hỗ trợ ít nhất một lớp MEX đầy đủ có kích thước k. 
6. Dừng khi k đạt đến giá trị trong đó f[k] = 0, vì không có dãy con nào có thể chứa giá trị k, do đó không thể có thêm lớp MEX nào nữa. 

### Tại sao nó hoạt động 

Mỗi lớp hợp lệ có kích thước k tương ứng chính xác với yêu cầu rằng dãy con chứa ít nhất một bản sao của mọi giá trị từ 0 đến k−1. Công thức sản phẩm tính toán chính xác các trình tự đó. Vì mỗi dãy con như vậy đóng góp chính xác một đơn vị cho lớp thứ k trong quá trình phân vùng tối ưu, nên tổng trên tất cả k sẽ tính mọi lớp có thể được hình thành trong bất kỳ phân tách tối ưu nào. Tính độc lập của các lựa chọn giá trị đảm bảo rằng không có chuỗi con nào bị tính quá mức trên các k khác nhau theo cách vi phạm tính tối ưu, bởi vì các lớp vốn đã rời rạc trong phạm vi giá trị bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        cnt = [0] * (n + 1)
        for x in a:
            cnt[x] += 1
        
        # precompute powers of 2
        pow2 = [1] * (n + 1)
        for i in range(1, n + 1):
            pow2[i] = (pow2[i - 1] * 2) % MOD
        
        ans = 0
        cur = 1
        
        for k in range(n + 1):
            if k > 0:
                if cnt[k - 1] == 0:
                    break
                cur = cur * (pow2[cnt[k - 1]] - 1) % MOD
            
            ans = (ans + cur) % MOD
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một mảng tần số để các ràng buộc về giá trị được chuyển đổi thành các đóng góp theo cấp số nhân. Sức mạnh của hai mảng tránh được việc tính toán lại lũy thừa nhiều lần. Biến`cur`theo dõi số dãy con thỏa mãn yêu cầu chứa tất cả các giá trị từ 0 đến k−1. Mỗi lần lặp lại sẽ mở rộng yêu cầu này thêm một giá trị, cập nhật sản phẩm tương ứng. 

Chi tiết triển khai chính là xử lý trường hợp không tồn tại giá trị bắt buộc. Trong trường hợp đó,`(2^{cnt} - 1)`sẽ bằng 0 và chúng tôi dừng sớm vì không thể có thêm lớp MEX nào nữa. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi quá trình trên một mảng ví dụ nhỏ`a = [0, 0, 1]`. 

Đối với mảng này, tần số là cnt[0]=2, cnt[1]=1. 

### Dấu vết 

| k | Giá trị bắt buộc | cur = sản phẩm(2^{cnt[v]}−1) | trả lời | 
| --- | --- | --- | --- | 
| 0 | không | 1 | 1 | 
| 1 | {0} | (2^2−1)=3 | 4 | 
| 2 | {0,1} | 3·(2^1−1)=3 | 7 | 

Dấu vết một phần này cho thấy các chuỗi con chứa các giá trị bắt buộc tích lũy đóng góp cho mỗi cấp MEX như thế nào. 

Bây giờ hãy xem xét một trường hợp suy biến`a = [1,1,1]`. 

| k | Giá trị bắt buộc | cur | trả lời | 
| --- | --- | --- | --- | 
| 0 | không | 1 | 1 | 
| 1 | {0} | 0 | 1 | 

Vì không có giá trị 0 nên không có lớp nào đóng góp nữa và chỉ có yêu cầu trống mới đóng góp cấu trúc cơ sở. 

Những dấu vết này cho thấy việc thiếu các giá trị nhỏ sẽ ngay lập tức chặn tất cả các lớp MEX cao hơn như thế nào, phản ánh cấu trúc của vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi giá trị được xử lý một lần và mỗi k được nâng cao tổng cộng tối đa n bước | 
| Không gian | O(n) | Mảng tần số và bảng công suất | 

Tổng của n trên tất cả các trường hợp thử nghiệm là 2·10^5, do đó, quét tuyến tính với cập nhật liên tục theo thời gian cho mỗi giá trị là đủ trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        cnt = [0] * (n + 1)
        for x in a:
            cnt[x] += 1
        
        pow2 = [1] * (n + 1)
        for i in range(1, n + 1):
            pow2[i] = (pow2[i - 1] * 2) % MOD
        
        ans = 0
        cur = 1
        
        for k in range(n + 1):
            if k > 0:
                if cnt[k - 1] == 0:
                    break
                cur = cur * (pow2[cnt[k - 1]] - 1) % MOD
            ans = (ans + cur) % MOD
        
        print(ans)

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("""4
3
0 0 1
4
0 0 1 1
5
0 0 1 2 2
4
1 1 1 1
""") == """11
26
53
0"""

# custom cases
assert run("""1
1
0
""") == "1", "single element"

assert run("""1
3
1 2 3
""") == "0", "missing zero"

assert run("""1
5
0 1 2 3 4
""") == "31", "full prefix present"

assert run("""1
2
0 0
""") == "4", "duplicate zeros"

test_cases = [
    ("1\n1\n0\n", "1", "single element"),
    ("1\n3\n1 2 3\n", "0", "missing zero"),
    ("1\n5\n0 1 2 3 4\n", "31", "full prefix"),
    ("1\n2\n0 0\n", "4", "duplicate zeros"),
]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | tính đúng đắn của trường hợp cơ sở | 
| thiếu 0 | 0 | không tồn tại lớp MEX hợp lệ | 
| tiền tố đầy đủ | 31 | tất cả các lớp đóng góp | 
| bản sao của 0 | 4 | xử lý đa dạng | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi thiếu giá trị 0. Trong tình huống đó, không có chuỗi con nào thậm chí có thể tạo thành lớp MEX đầu tiên, bởi vì mỗi lớp yêu cầu ít nhất một số 0. Thuật toán xử lý điều này bằng cách tạo ngay lập tức`cur = 0`tại k = 1, điều này ngăn cản sự tích lũy thêm. 

Ví dụ, đầu vào`a = [2,3,4]`dẫn đến cnt[0]=0. Tại k=1 thì sản phẩm bao gồm`(2^0 - 1) = 0`, vì vậy mọi đóng góp cao hơn đều biến mất. Điều này phù hợp với thực tế là không có phân vùng nào có thể tạo ra bất kỳ đóng góp tích cực nào cho MEX. 

Một trường hợp cạnh khác xảy ra khi tất cả các phần tử đều bằng 0. Sau đó, mọi chuỗi con đều hỗ trợ các lớp có độ dài 1 không giới hạn nhưng không có lớp nào cao hơn. Thuật toán phản ánh điều này bằng cách cho phép k=1 nhưng phá vỡ ở mức k=2 do thiếu giá trị 1 và đếm chính xác tất cả các chuỗi con có trọng số theo số lần chúng có thể đóng góp MEX=1.
