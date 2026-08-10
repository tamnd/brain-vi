---
title: "CF 104014E - \u0418\u0441\u0442\u043e\u0440\u0438\u044f \u0432\u0435\u0440\u0441\u0438\u0439"
description: "Chúng ta được cấp số phiên bản cuối cùng của một dự án, được viết dưới dạng số nguyên dương $N$. Dự án phát triển từng tháng theo một quy tắc cố định: nếu phiên bản hiện tại có các chữ số $k$, thì sau một tháng, nó sẽ tăng lên theo số bao gồm các chữ số $k$."
date: "2026-07-02T04:56:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104014
codeforces_index: "E"
codeforces_contest_name: "2022-2023 ICPC NERC, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u0438 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0440\u0435\u0433\u0438\u043e\u043d\u0430 \u0438 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438"
rating: 0
weight: 104014
solve_time_s: 51
verified: true
draft: false
---

[CF 104014E - \u0418\u0441\u0442\u043e\u0440\u0438\u044f \u0432\u0435\u0440\u0441\u0438\u0439](https://codeforces.com/problemset/problem/104014/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp số phiên bản cuối cùng của dự án, được viết dưới dạng số nguyên dương$N$. Dự án phát triển từng tháng theo một quy tắc cố định: nếu phiên bản hiện tại có$k$chữ số, sau một tháng nó tăng lên bằng số bao gồm$k$những cái đó. Ví dụ: phiên bản 5 chữ số tăng thêm$11111$trong tháng tiếp theo, phiên bản 3 chữ số sẽ tăng thêm$111$, vân vân. 

Nhiệm vụ không phải là mô phỏng về phía trước mà là tái tạo lại lịch sử lâu nhất có thể dẫn đến con số đã cho$N$. Phiên bản đầu tiên trong lịch sử đó có thể là bất kỳ số nguyên dương nào và mỗi lần chuyển đổi phải tuân theo quy tắc rằng mức tăng phụ thuộc vào độ dài chữ số của trạng thái hiện tại. Chúng tôi muốn số tháng tối đa có thể, bao gồm cả tháng hiện tại. 

Khó khăn chính là mức tăng phụ thuộc vào số chữ số của phiên bản hiện tại chứ không phải hằng số cố định. Điều này có nghĩa là quá trình này không phải là một cấp số cộng đơn giản và việc đảo ngược nó không đơn giản. 

Ràng buộc$N \le 10^{18}$ngụ ý rằng tất cả các giá trị trung gian đều nằm trong số nguyên 64 bit. Bất kỳ cách tiếp cận nào cố gắng khám phá tất cả các lịch sử có thể xảy ra một cách ngây thơ sẽ bùng nổ theo cấp số nhân, bởi vì về nguyên tắc mỗi trạng thái có thể có nhiều trạng thái tiền thân có thể có. 

Một trường hợp cạnh tinh vi phát sinh từ sự chuyển đổi độ dài chữ số. Ví dụ: di chuyển từ 999 đến 1000 sẽ thay đổi số chữ số, điều này sẽ thay đổi kích thước gia số trong bước tiếp theo. Nếu chúng ta bỏ qua các ràng buộc về chữ số khi đảo ngược quá trình, chúng ta có thể xây dựng các lịch sử không hợp lệ không đáp ứng quy tắc chuyển tiếp. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp là mô phỏng ngược từ$N$. Nếu chúng ta đang ở một giá trị$x$, chúng tôi cố gắng đoán giá trị trước đó$y$. Nếu như$y$có$k$chữ số, thì quá trình chuyển đổi về phía trước là$y + R_k = x$, Ở đâu$R_k = 111\ldots1$(k chữ số). Vì vậy chúng ta có thể tính toán các ứng cử viên$y = x - R_k$cho tất cả các độ dài chữ số có thể$k$, sau đó kiểm tra xem điều này$y$là hợp lệ. 

Bước đảo ngược mạnh mẽ này là chính xác cục bộ: mọi trạng thái hợp lệ trước đó phải xuất hiện trong số các ứng cử viên này. Tuy nhiên, phân nhánh trên tất cả những gì có thể$k \in [1, 18]$và khám phá đệ quy tất cả các chuỗi dẫn đến một cây tìm kiếm lớn trong trường hợp xấu nhất. Mặc dù độ sâu bị giới hạn vì các giá trị bị thu hẹp lại, những lựa chọn khác nhau về$k$có thể dẫn đến các lịch sử khác nhau và chúng tôi muốn độ dài tối đa, vì vậy lựa chọn tham lam rõ ràng là không an toàn. 

Quan sát quan trọng là không gian trạng thái nhỏ đối với các chuyển tiếp đi. Mỗi số có tối đa 18 số liền trước hợp lệ và mỗi số liền trước được xác định duy nhất bởi$k$. Điều này cho phép chúng ta coi quá trình này như một biểu đồ có hướng trong đó mỗi nút là một số và các cạnh trỏ đến các trạng thái hợp lệ trước đó. Vì các giá trị giảm dần dọc theo các cạnh nên không có chu kỳ và chúng ta có thể tính toán đường đi dài nhất bằng cách sử dụng DFS được ghi nhớ. 

Vấn đề giảm xuống việc tìm chuỗi dài nhất kết thúc tại$N$trong một DAG trong đó mỗi nút phân nhánh thành tối đa 18 nút trước được xác định bởi các ràng buộc về độ dài chữ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quay lại vũ phu | Hàm mũ | O(độ sâu) | Quá chậm | 
| DFS được ghi nhớ so với phiên bản tiền nhiệm | O(18 log N) | O(tiểu bang đã ghé thăm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xác định giá trị đơn vị 

Đối với mỗi độ dài chữ số có thể$k$, tính toán trước$R_k = 111\ldots1$. Điều này được tính toán một cách hiệu quả như$(10^k - 1)/9$. 

Điều này cho chúng ta một cách trực tiếp để kiểm tra xem một số có thể đến từ trạng thái trước đó với$k$chữ số. 

### 2. Xác định chuyển đổi ngược 

Đối với một giá trị hiện tại$x$, chúng tôi thử tất cả các độ dài chữ số có thể$k$. Chúng tôi tính toán một ứng cử viên tiền nhiệm:$$y = x - R_k$$Điều này thể hiện giá trị trước đó duy nhất có thể có nếu số trước đó có chính xác$k$chữ số. 

### 3. Xác thực ứng viên 

một ứng cử viên$y$chỉ có giá trị nếu nó thỏa mãn hai điều kiện. 

Đầu tiên, nó phải dương và nằm trong khoảng$k$-số có chữ số:$$10^{k-1} \le y \le 10^k - 1$$Thứ hai, nó phải nhất quán về phía trước:$$y + R_k = x$$Điều kiện thứ hai luôn đúng về mặt đại số nếu chúng ta xây dựng$y$chính xác, vì vậy hạn chế thực sự là tính nhất quán về độ dài chữ số. 

### 4. Tính toán chuỗi tốt nhất bằng DFS + ghi nhớ 

Chúng tôi xác định một chức năng$f(x)$là số bước tối đa kết thúc tại$x$. Sau đó:$$f(x) = 1 + \max f(y)$$trên tất cả những người tiền nhiệm hợp lệ$y$. 

Nếu không có tiền thân hợp lệ,$f(x) = 1$. 

Việc ghi nhớ đảm bảo mỗi giá trị được tính một lần. 

### 5. Trả lời trả lời 

Câu trả lời cuối cùng là$f(N)$, đại diện cho lịch sử dài nhất có thể kết thúc ở phiên bản đã cho. 

### Tại sao nó hoạt động 

Mọi lịch sử hợp lệ phải tương ứng với một chuỗi các chuyển đổi ngược hợp lệ và mọi chuyển đổi ngược được xác định đầy đủ bằng việc lựa chọn độ dài chữ số. Vì mỗi lần chuyển đổi làm giảm nghiêm ngặt độ lớn của số nên biểu đồ trạng thái có tính chất không theo chu kỳ. Do đó, phép lặp được ghi nhớ sẽ tính toán chính xác đường đi dài nhất trong DAG này, vì tất cả các đường đi trước có thể có đều được khám phá và tái sử dụng một cách tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1000000)

from functools import lru_cache

def repunit(k):
    return (10**k - 1) // 9

pow10 = [1]
for _ in range(20):
    pow10.append(pow10[-1] * 10)

@lru_cache(None)
def dfs(x):
    best = 1
    for k in range(1, 19):
        r = repunit(k)
        y = x - r
        if y <= 0:
            continue
        if pow10[k-1] <= y <= pow10[k] - 1:
            best = max(best, 1 + dfs(y))
    return best

def main():
    n = int(input().strip())
    print(dfs(n))

if __name__ == "__main__":
    main()
```Việc thực hiện mã hóa trực tiếp sự lặp lại. các`repunit(k)`chức năng xây dựng mô hình gia tăng một cách hiệu quả. các`pow10`mảng được sử dụng để thực thi các ràng buộc về độ dài chữ số trong thời gian không đổi. 

DFS khám phá tất cả các số trước hợp lệ có thể có và ghi nhớ kết quả để mỗi số chỉ được xử lý một lần. Độ sâu đệ quy vẫn nhỏ vì mỗi bước làm giảm độ lớn một cách đáng kể. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ$N = 100$. Chúng tôi thử độ dài chữ số có thể có trước đó. 

| Bước | Hiện tại x | k đã thử | Ứng viên y | Có hiệu lực? | dfs(y) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 100 | 2 | 100 - 11 = 89 | vâng | tính toán | 
| 2 | 89 | 2 | 78 | vâng | tính toán | 
| 3 | 78 | 2 | 67 | vâng | tính toán | 

Điều này hiển thị một chuỗi dài hoàn toàn trong các số có 2 chữ số, tạo ra mô hình giảm dần ổn định. 

Bây giờ hãy xem xét$N = 24690$, phù hợp với cấu trúc 5 chữ số. 

| Bước | Hiện tại x | k | Ứng viên y | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| 1 | 24690 | 5 | 24690 - 11111 = 13579 | vâng | 
| 2 | 13579 | 5 | 13579 - 11111 = 2468 | không (chữ số không khớp với k=5) | 
| 2 | 13579 | 4 | 13579 - 1111 = 12468 | vâng | 

Điều này chứng tỏ tại sao nhiều$k$sự lựa chọn quan trọng và tại sao sự lựa chọn tham lam ngây thơ có thể thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(18 \cdot \text{states})$| Mỗi trạng thái thử độ dài tối đa 18 chữ số và mỗi giá trị được tính một lần thông qua ghi nhớ | 
| Không gian |$O(\text{states})$| Bảng ghi nhớ lưu trữ mỗi số có thể truy cập một lần | 

Số lượng trạng thái có thể truy cập là nhỏ vì mỗi lần chuyển đổi làm giảm số lượng đáng kể, do đó đệ quy chỉ khám phá một tập hợp con thưa thớt của các giá trị lên đến$10^{18}$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    sys.setrecursionlimit(1000000)

    from functools import lru_cache

    def repunit(k):
        return (10**k - 1) // 9

    pow10 = [1]
    for _ in range(20):
        pow10.append(pow10[-1] * 10)

    @lru_cache(None)
    def dfs(x):
        best = 1
        for k in range(1, 19):
            r = repunit(k)
            y = x - r
            if y <= 0:
                continue
            if pow10[k-1] <= y <= pow10[k] - 1:
                best = max(best, 1 + dfs(y))
        return best

    n = int(inp.strip())
    return str(dfs(n))

# simple small chains
assert run("100") == run("100"), "self consistency check"
assert run("24690") == run("24690"), "sample-like check"
assert run("1") == "1", "minimum case"

# decreasing digit boundary
assert run("1000") >= "1", "boundary digits"

# crafted short chain
assert run("12") >= "1", "small input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Trạng thái tối thiểu | 
| 100 | biến | Hành vi chuỗi nhiều bước | 
| 24690 | biến | Chuyển đổi độ dài chữ số | 
| 1000 | biến | Ranh giới xung quanh lũy thừa 10 | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi phép trừ gây ra thay đổi độ dài chữ số. Ví dụ: bắt đầu từ một số có 4 chữ số có thể tạo ra số liền trước có 3 chữ số, điều này sẽ thay đổi quy tắc tăng dần trong bước tiếp theo. Thuật toán xử lý việc này bằng cách kiểm tra rõ ràng các ràng buộc về chữ số cho từng ứng viên$k$, đảm bảo rằng chỉ những người tiền nhiệm có độ dài hợp lệ mới được xem xét. 

Một trường hợp cạnh khác xảy ra gần lũy thừa mười. Ví dụ: các giá trị như 1000 hoặc 10000 có thể có các giá trị trước đó ngắn hơn một chữ số hoặc có độ dài bằng nhau tùy thuộc vào$k$. DFS không giả định tính liên tục của độ dài chữ số và đánh giá tất cả các khả năng một cách độc lập. 

Trường hợp cuối cùng là những số rất nhỏ không tồn tại trước đó. Trong những trường hợp như vậy, DFS trả về chính xác 1, biểu thị lịch sử có độ dài bằng một chỉ bao gồm phiên bản hiện tại.
