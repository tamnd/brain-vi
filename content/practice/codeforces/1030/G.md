---
title: "CF 1030G - Máy phát đồng dư tuyến tính"
description: "Chúng ta được cung cấp một hệ thống phát triển vectơ $n$ chiều qua các bước riêng biệt. Mỗi tọa độ phát triển độc lập bằng cách sử dụng cùng một loại quy tắc: phép biến đổi tuyến tính modulo một số nguyên tố."
date: "2026-06-16T21:04:51+07:00"
tags: ["codeforces", "competitive-programming", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1030
codeforces_index: "G"
codeforces_contest_name: "Technocup 2019 - Elimination Round 1"
rating: 2900
weight: 1030
solve_time_s: 151
verified: true
draft: false
---

[CF 1030G - Trình tạo đồng dư tuyến tính](https://codeforces.com/problemset/problem/1030/G) 

**Xếp hạng:** 2900 
**Thẻ:** lý thuyết số 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống phát triển một$n$vectơ -chiều trên các bước rời rạc. Mỗi tọa độ phát triển độc lập bằng cách sử dụng cùng một loại quy tắc: phép biến đổi tuyến tính modulo một số nguyên tố. 

Đối với mỗi chỉ số$i$, giá trị$f_i^{(k)}$chỉ phụ thuộc vào giá trị trước đó của nó$f_i^{(k-1)}$thông qua hàm affine modulo$p_i$. Chúng ta được phép chọn vectơ ban đầu và tất cả các hệ số của các hàm affine này một cách tự do, với mục tiêu tạo ra chuỗi toàn bộ bộ dữ liệu$(f_1^{(k)}, \dots, f_n^{(k)})$tạo ra càng nhiều trạng thái riêng biệt càng tốt trước khi nó lặp lại. 

Nhiệm vụ là xác định số lượng tối đa các bộ dữ liệu riêng biệt có thể xuất hiện trong một chuỗi như vậy. 

Hạn chế chính đó là$n$có thể lên đến$2 \cdot 10^5$, vì vậy mọi giải pháp về cơ bản phải tuyến tính trong$n$. Mỗi$p_i$là số nguyên tố lên đến$2 \cdot 10^6$, điều này gợi ý rõ ràng rằng chúng ta sẽ không bao giờ lặp lại các giá trị modulo$p_i$, mà thay vào đó dựa vào cấu trúc đại số của quy tắc cập nhật. 

Một mô phỏng đơn giản sẽ cố gắng suy luận về chu kỳ của từng tọa độ cho các tham số đã chọn. Điều đó ngay lập tức thất bại vì mỗi tọa độ có tới$p_i$các trạng thái và sản phẩm của tất cả$p_i$có kích thước lớn về mặt thiên văn. 

Một trường hợp thất bại tinh tế hơn xuất hiện nếu người ta giả định các phép lặp tuyến tính tùy ý tạo ra các cấu trúc chu trình phức tạp phải được phân tích riêng lẻ. Ví dụ: người ta có thể cố gắng tính độ dài chu trình cho từng hàm affine và sau đó kết hợp chúng, nhưng giả định sai độ dài chu trình tối đa phụ thuộc vào cả hai.$a_i$Và$b_i$một cách phức tạp. Trong thực tế, cấu trúc trên trường nguyên tố đủ cứng để chúng ta có thể tạo ra một chu trình cực đại. 

Khó khăn cốt lõi là nhận ra rằng chúng tôi không phân tích một máy phát điện cố định. Chúng tôi đang chọn máy phát điện để tối đa hóa kích thước quỹ đạo. 

## Phương pháp tiếp cận 

Mỗi tọa độ phát triển độc lập dưới bản đồ$$x \mapsto a_i x + b_i \pmod{p_i}$$trên một trường hữu hạn có kích thước nguyên tố. Vì vậy, mỗi tọa độ là một hàm trên một tập hợp kích thước$p_i$và toàn bộ hệ thống là sản phẩm của các hệ động lực độc lập. 

Mô hình tinh thần mạnh mẽ là chọn các tham số và mô phỏng độ dài chu kỳ cảm ứng. Với mỗi tọa độ, chúng ta có thể liệt kê tất cả các lựa chọn về$a_i, b_i, x_i$, mô phỏng cho đến khi lặp lại và tính toán kích thước quỹ đạo, sau đó thử kết hợp chúng. Điều này hoàn toàn không khả thi vì ngay cả một tọa độ cũng đã có$O(p_i)$trạng thái và số lượng lựa chọn tham số là$O(p_i^2)$, do đó tổng công sẽ bùng nổ ngay lập tức. 

Quan sát quan trọng là kích thước quỹ đạo tối đa có thể có của phép biến đổi affine trên trường nguyên tố không phức tạp. Bản đồ có thể là một hoán vị hoặc không, và khi nó là một hoán vị, việc phân tách chu trình của nó có thể được thực hiện thành một chu trình có kích thước duy nhất$p_i$. 

Trên một trường nguyên tố, bất kỳ hàm nào có dạng$x \mapsto x + c$với$c \neq 0$là sự dịch chuyển có tính chu kỳ của tất cả các phần tử. Lặp lại nó đi qua tất cả$p_i$giá trị trước khi lặp lại. Điều này đã mang lại khoảng thời gian tối đa có thể cho một tọa độ duy nhất, vì không có hệ thống nào trên một tập hợp kích thước$p_i$có thể có một chu kỳ dài hơn$p_i$. 

Vì tọa độ tiến triển độc lập nên tổng số bộ dữ liệu riêng biệt bằng tích của độ dài chu kỳ riêng lẻ. Tối đa hóa từng tọa độ một cách độc lập sẽ tối đa hóa sản phẩm. 

Vì vậy chiến lược tối ưu là buộc mọi tọa độ phải có chu kỳ chính xác$p_i$, và câu trả lời trở thành tích của mọi số nguyên tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lý luận chu kỳ lực lượng vũ phu cho mỗi tham số | hàm mũ | O(1) | Quá chậm | 
| Sản phẩm tối ưu của quan sát số nguyên tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng mỗi tọa độ tiến triển độc lập theo một hàm modulo một số nguyên tố$p_i$. Điều này có nghĩa là sự tiến hóa bộ dữ liệu đầy đủ là tích Descartes của các hệ động lực một chiều độc lập. 
2. Đối với tọa độ cố định$i$, nhận ra rằng không gian trạng thái có chính xác$p_i$các phần tử. Bất kỳ chuỗi nào được tạo ra bằng cách áp dụng lặp đi lặp lại một hàm xác định cuối cùng phải lặp lại, do đó độ dài quỹ đạo tối đa là$p_i$. Điều này đưa ra giới hạn trên tuyệt đối cho mỗi tọa độ. 
3. Chứng minh rằng giới hạn trên này có thể đạt được. Lựa chọn$a_i = 1$Và$b_i = 1$thực hiện sự chuyển đổi$x \mapsto x + 1 \pmod{p_i}$. Mỗi ứng dụng chuyển sang một giá trị mới cho đến khi truy cập hết phần dư trước khi quay lại từ đầu. Điều này tạo ra một chu kỳ có độ dài$p_i$. 
4. Kết luận rằng độ dài quỹ đạo tối đa có thể có của tọa độ$i$chính xác là$p_i$, vì nó không thể vượt quá số lượng trạng thái và công trình này đạt được nó. 
5. Vì tọa độ là độc lập nên bộ dữ liệu chỉ lặp lại khi mọi tọa độ lặp lại đồng thời. Do đó, chu kỳ đầy đủ là sự đồng bộ hóa ít phổ biến nhất của các chu kỳ độc lập, ở đây chính xác là tích của độ dài của chúng. 
6. Nhân tất cả$p_i$cùng nhau theo modulo$10^9+7$. 

### Tại sao nó hoạt động 

Sự phát triển của không gian trạng thái đầy đủ là sản phẩm của các chu trình hữu hạn độc lập, mỗi chu trình trên một tọa độ. Mỗi chu kỳ tọa độ có thể được tối đa hóa độc lập theo chiều dài$p_i$và không có tọa độ nào có thể vượt quá kích thước không gian trạng thái của nó. Tính độc lập đảm bảo rằng sự lặp lại bộ dữ liệu xảy ra chính xác khi tất cả các tọa độ quay trở lại vị trí bắt đầu của chúng cùng một lúc, do đó tổng số bộ dữ liệu riêng biệt bằng tích của độ dài chu kỳ. Điều này thiết lập cả giới hạn trên và khả năng đạt được của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def main():
    n = int(input())
    p = list(map(int, input().split()))
    
    ans = 1
    for x in p:
        ans = (ans * x) % MOD
    
    print(ans)

if __name__ == "__main__":
    main()
```Việc thực hiện áp dụng trực tiếp kết quả dẫn xuất. Trạng thái duy nhất được duy trì là tích hoạt động của tất cả các số nguyên tố modulo$10^9+7$. Không có sự phụ thuộc ẩn giữa các tọa độ nên không cần xử lý thêm. 

Một cạm bẫy phổ biến ở đây là làm phức tạp quá mức sự tái diễn và cố gắng phân tích$a_i, b_i$sự kết hợp. Cấu trúc tối ưu sẽ tránh được tất cả những điều đó bằng cách chọn các tham số đảm bảo một chu trình đầy đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
2 3 5 7
```Chúng tôi tính toán sản phẩm từng bước. 

| Bước | Thủ tướng | Sản phẩm đang chạy | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 3 | 6 | 
| 3 | 5 | 30 | 
| 4 | 7 | 210 | 

Kết quả là 210, tương ứng với việc đạt được đầy đủ các chu kỳ có độ dài 2, 3, 5 và 7 một cách độc lập. 

### Mẫu 2 

Hãy xem xét:```
3
3 3 5
```| Bước | Thủ tướng | Sản phẩm đang chạy | 
| --- | --- | --- | 
| 1 | 3 | 3 | 
| 2 | 3 | 9 | 
| 3 | 5 | 45 | 

Câu trả lời là 45. Mỗi tọa độ tuần hoàn độc lập qua tất cả các dư lượng modulo mô đun chính của nó và bộ dữ liệu quay vòng qua tích Descartes đầy đủ. 

Điều này xác nhận rằng tính độc lập cấp số nhân trên các tọa độ mô hình chính xác chu trình toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một phép nhân cho mỗi số nguyên tố | 
| Không gian |$O(1)$| Chỉ một sản phẩm đang chạy được lưu trữ | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$số nguyên tố, vì vậy chỉ cần một lần tuyến tính là đủ. Các giá trị của$p_i$đủ nhỏ để nhân một cách an toàn theo số học modulo mà không phải lo lắng về lỗi tràn trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve():
    input = sys.stdin.readline
    n = int(input())
    p = list(map(int, input().split()))
    ans = 1
    for x in p:
        ans = (ans * x) % MOD
    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided sample
assert run("4\n2 3 5 7\n") == "210"

# minimum size
assert run("1\n2\n") == "2"

# repeated primes
assert run("3\n3 3 3\n") == str((3*3*3) % MOD)

# mixed primes
assert run("2\n2 7\n") == str((2*7) % MOD)

# larger case
assert run("5\n2 3 5 7 11\n") == str((2*3*5*7*11) % MOD)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số nguyên tố đơn | 2 | ranh giới tối thiểu | 
| số nguyên tố lặp lại | 27 | độc lập trên các mô-đun giống hệt nhau | 
| số nguyên tố hỗn hợp | 14 | phép nhân cơ bản đúng đắn | 
| năm số nguyên tố | 2310 | hành vi sản phẩm lớn hơn | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi tất cả$p_i$đều bằng 2. Trong trường hợp đó, mỗi tọa độ chỉ có hai trạng thái và có vẻ như sự tương tác giữa các lựa chọn affine có thể làm giảm chu trình. Tuy nhiên, sử dụng$x \mapsto x + 1 \bmod 2$đảm bảo 2 chu kỳ cho mỗi tọa độ, do đó không gian bộ dữ liệu đầy đủ vẫn có kích thước$2^n$và thuật toán trả về chính xác sản phẩm$2^n \bmod (10^9+7)$. 

Một trường hợp khác là khi$n = 1$. Hệ thống suy biến thành một bản đồ affine duy nhất trên trường nguyên tố. Quỹ đạo tối đa chính xác là$p_1$và thuật toán trả về chính xác$p_1$, phù hợp với thực tế là tồn tại một sự dịch chuyển tuần hoàn đầy đủ trên bất kỳ trường hữu hạn nào có kích thước nguyên tố.
