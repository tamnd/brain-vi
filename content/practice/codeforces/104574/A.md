---
title: "CF 104574A - Sân chơi Kỳ nhông"
description: "Chúng ta có hai tờ giấy hình chữ nhật giống hệt nhau, mỗi tờ có kích thước $M nhân N$. Chúng ta được phép cắt từng tờ giấy thành những hình chữ nhật nhỏ hơn, thẳng hàng với trục bằng cách sử dụng các đường cắt thẳng song song với các cạnh."
date: "2026-06-30T08:15:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104574
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 2 (Beginner)"
rating: 0
weight: 104574
solve_time_s: 67
verified: true
draft: false
---

[CF 104574A - Sân chơi Iguana](https://codeforces.com/problemset/problem/104574/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tờ giấy hình chữ nhật giống hệt nhau, mỗi tờ có kích thước$M \times N$. Chúng ta được phép cắt từng tờ giấy thành những hình chữ nhật nhỏ hơn, thẳng hàng với trục bằng cách sử dụng các đường cắt thẳng song song với các cạnh. Sau khi cắt, tất cả các mảnh thu được từ cả hai tấm có thể được sắp xếp lại và xoay tự do (ngầm định, vì hướng không quan trọng khi hình thành một hình chữ nhật mới từ các mảnh) và chúng tôi muốn lắp ráp chúng thành một hình chữ nhật mục tiêu duy nhất có kích thước$P \times Q$. 

Không giống như nhiều vấn đề về ốp lát, chúng ta không bắt buộc phải sử dụng tất cả vật liệu. Bất kỳ phần còn lại nào không cần thiết để tạo thành hình chữ nhật mục tiêu vẫn chưa được sử dụng và chúng ta phải tính tổng diện tích của chúng. Nếu hoàn toàn không thể xây dựng hình chữ nhật mục tiêu, chúng ta sẽ xuất ra "KHÔNG THỂ". 

Quan sát quan trọng là hạn chế thực sự duy nhất đến từ tổng diện tích sẵn có. Mỗi hình chữ nhật ban đầu đóng góp$M \cdot N$, vậy cùng nhau chúng ta có$2MN$đơn vị bình phương giá trị của vật liệu. Vì cho phép cắt theo trục tùy ý nên mỗi hình chữ nhật có thể được tinh chỉnh xuống thành hình vuông đơn vị, nghĩa là chúng ta thực sự có$2MN$các ô 1x1 độc lập có thể được sắp xếp lại thành bất kỳ hình chữ nhật nào miễn là thỏa mãn giới hạn diện tích. 

Những hạn chế$M, N, P, Q \leq 1000$ngụ ý rằng tất cả các phép tính đều phù hợp thoải mái trong số nguyên 32 bit, vì vậy chúng ta không cần phải lo lắng về việc tràn trong Python. Giải pháp phải chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một lỗi phổ biến là suy nghĩ quá nhiều về các ràng buộc hình học, chẳng hạn như cố gắng khớp các tỷ lệ khung hình hoặc mô phỏng các chiến lược cắt. Ví dụ, người ta có thể giả định không chính xác rằng vì chúng ta bắt đầu chỉ với hai hình chữ nhật nên mục tiêu bằng cách nào đó phải căn chỉnh với$M$hoặc$N$. Điều này dẫn đến việc từ chối sai như: 

đầu vào:```
4 4
5 3
```Cách tiếp cận hình học đơn giản có thể thất bại vì 5 không chia cho 4 hoặc ngược lại, tuy nhiên câu trả lời đúng vẫn có giá trị vì chúng ta chỉ quan tâm đến tổng diện tích. 

Một trường hợp thất bại khác là: 

đầu vào:```
2 2
3 3
```Ở đây, tổng diện tích là 8 trong khi mục tiêu là 9, vì vậy điều đó là không thể dù hình dạng trông giống nhau. 

## Phương pháp tiếp cận 

Tư duy vũ phu sẽ cố gắng mô phỏng tất cả các kiểu cắt có thể có của hai hình chữ nhật, chia chúng một cách đệ quy và cố gắng lắp ráp hình chữ nhật mục tiêu. Điều này nhanh chóng bùng nổ vì mỗi lần cắt sẽ làm tăng số lượng mảnh và mỗi mảnh có thể được phân chia lại theo nhiều cách. Ngay cả khi chúng ta hạn chế cắt theo tọa độ nguyên, số lượng phân vùng của hình chữ nhật vẫn tăng theo cấp số nhân ở cả hai chiều, khiến phương pháp này không khả thi ngay cả đối với$M, N \leq 1000$. 

Cái nhìn sâu sắc quan trọng là quyền tự do cắt giúp loại bỏ các ràng buộc về cấu trúc. Vì chúng ta có thể cắt dọc theo các đường lưới nhiều lần, mỗi đường$M \times N$hình chữ nhật có thể được giảm thành$MN$đơn vị hình vuông. Với hai hình chữ nhật, chúng ta thực sự có nhiều tập hợp$2MN$đơn vị hình vuông. Sau khi giảm xuống mức này, yêu cầu duy nhất còn lại để tạo thành hình chữ nhật mục tiêu là có đủ tổng số ô vuông đơn vị. 

Điều này làm giảm toàn bộ vấn đề thành một so sánh diện tích đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng cắt Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Kiểm tra theo khu vực | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng diện tích có sẵn là$2 \cdot M \cdot N$. Điều này thể hiện số lượng bình phương đơn vị tối đa mà chúng ta có thể trích xuất từ ​​hai hình chữ nhật sau khi cắt tùy ý. 
2. Tính diện tích cần tìm là$P \cdot Q$. Đây là số ô vuông đơn vị chính xác cần thiết để tạo thành hình chữ nhật mục tiêu. 
3. So sánh diện tích cần thiết với diện tích có sẵn. Nếu diện tích yêu cầu vượt quá diện tích có sẵn thì không thể xây dựng hình chữ nhật mục tiêu vì không có chiến lược sắp xếp lại hoặc cắt nào có thể tạo ra nhiều vật liệu hơn số lượng hiện có. 
4. Nếu có thể xây dựng, hãy tính diện tích còn lại là$2MN - PQ$, tương ứng với các ô vuông đơn vị chưa sử dụng sau khi hình thành mục tiêu. 

### Tại sao nó hoạt động 

Bởi vì các đường cắt thẳng tùy ý cho phép chúng ta chia nhỏ cả hai hình chữ nhật ban đầu thành các ô 1x1 đơn vị, nên hình dạng ban đầu không áp đặt ràng buộc nào ngoài tổng số lượng các ô này. Bất kỳ hình chữ nhật nào có độ dài cạnh nguyên chỉ đơn giản là sự sắp xếp khác nhau của cùng một ô đơn vị. Vì việc sắp xếp lại không bị hạn chế nên tính khả thi chỉ phụ thuộc vào việc có tồn tại đủ ô để bao phủ vùng mục tiêu hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    M, N = map(int, input().split())
    P, Q = map(int, input().split())

    total = 2 * M * N
    need = P * Q

    if need > total:
        print("IMPOSSIBLE")
    else:
        print(total - need)

if __name__ == "__main__":
    solve()
```Giải pháp đọc hai hình chữ nhật và tính toán trực tiếp tổng diện tích có sẵn và diện tích cần thiết. Chi tiết triển khai chính là giữ mọi thứ ở dạng số học số nguyên mà không có bất kỳ mô phỏng hình học nào. Phép trừ chỉ được thực hiện sau khi kiểm tra tính khả thi để tránh tạo ra các giá trị âm còn sót lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
M N = 4 4
P Q = 5 3
```| Bước | Tổng diện tích | Khu vực cần thiết | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 32 | 15 | tiến hành | 
| 2 | 32 >= 15 | vâng | xây dựng khả thi | 
| 3 | còn sót lại = 17 | | đầu ra | 

Điều này cho thấy rằng mặc dù về mặt cấu trúc 5x3 không liên quan đến 4x4 nhưng diện tích vẫn đủ, do đó cách xây dựng là hợp lệ. 

### Mẫu 2 

đầu vào:```
M N = 2 2
P Q = 3 3
```| Bước | Tổng diện tích | Khu vực cần thiết | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 8 | 9 | dừng lại | 
| 2 | 8 < 9 | không | không thể | 

Điều này khẳng định rằng tổng diện tích không đủ sẽ ngay lập tức cản trở mọi hoạt động xây dựng, bất kể chiến lược cắt giảm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học và so sánh được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp này thỏa mãn một cách tầm thường các ràng buộc vì tất cả các đầu vào được giới hạn bởi 1000 và việc tính toán chỉ liên quan đến số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    M, N = map(int, input().split())
    P, Q = map(int, input().split())
    total = 2 * M * N
    need = P * Q
    if need > total:
        print("IMPOSSIBLE")
    else:
        print(total - need)

# provided samples
assert run("4 4\n5 3\n") == "17", "sample 1"
assert run("2 2\n3 3\n") == "IMPOSSIBLE", "sample 2"

# custom cases
assert run("1 1\n1 1\n") == "1", "minimum equal case"
assert run("1 1\n2 1\n") == "IMPOSSIBLE", "insufficient area by width"
assert run("10 10\n10 10\n") == "0", "exact fit uses all area"
assert run("3 7\n4 5\n") == str(2*3*7 - 20), "random feasible case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 / 1 1 | 1 | trường hợp khả thi không tầm thường nhỏ nhất | 
| 1 1 / 2 1 | KHÔNG THỂ | sự cố ranh giới do diện tích | 
| 10 10 / 10 10 | 0 | sử dụng đầy đủ chính xác | 
| 3 7 / 4 5 | 22 | tính khả thi ngẫu nhiên chung | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mục tiêu khớp chính xác với một trong các hình chữ nhật ban đầu. Ví dụ, nếu$M=N=5$Và$P=5, Q=5$, thuật toán tính tổng diện tích$50$, diện tích cần thiết$25$, và trả về$25$. Điều này tương ứng với việc chỉ sử dụng một nửa vật liệu, điều này hợp lệ vì những phần còn sót lại được cho phép. 

Một trường hợp cạnh khác là khi diện tích mục tiêu bằng tổng diện tích có sẵn. Nếu như$M=N=3$Và$P=3, Q=6$, tổng diện tích là$18$và diện tích cần thiết cũng là$18$. Thuật toán xuất ra 0, nghĩa là không có gì bị lãng phí. Điều này xác nhận rằng việc sử dụng toàn bộ được xử lý rõ ràng mà không cần phân nhánh đặc biệt. 

Trường hợp cạnh cuối cùng là khi một chiều cực kỳ lớn so với hình chữ nhật ban đầu, chẳng hạn như$M=N=1000$,$P=1$,$Q=1$. Thuật toán vẫn hoạt động chính xác vì nó chỉ so sánh diện tích và kết quả trở thành$2 \cdot 10^6 - 1$, hợp lệ.
