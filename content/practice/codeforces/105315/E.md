---
title: "CF 105315E - Sinh Nhật Rama"
description: "Chúng ta được cung cấp một chuỗi mô tả thông tin gcd tích lũy của một mảng ẩn khác. Với mỗi vị trí $i$, giá trị $bi$ bằng gcd của phần tử $i$ đầu tiên của một mảng $a$ chưa biết."
date: "2026-06-23T15:06:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105315
codeforces_index: "E"
codeforces_contest_name: "JPC 4.0"
rating: 0
weight: 105315
solve_time_s: 50
verified: true
draft: false
---

[CF 105315E - Sinh nhật của Rama](https://codeforces.com/problemset/problem/105315/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi mô tả thông tin gcd tích lũy của một mảng ẩn khác. Đối với mỗi vị trí$i$, giá trị$b_i$bằng gcd của số đầu tiên$i$các phần tử của một mảng chưa biết$a$. Nhiệm vụ của chúng ta là xây dựng lại bất kỳ mảng hợp lệ nào$a$có thể tạo ra chuỗi đã cho$b$hoặc xác định rằng không tồn tại mảng như vậy. 

Khó khăn chính là gcd nói chung có tính tích lũy và không thể đảo ngược. Mỗi$b_i$ràng buộc tất cả các phần tử trước đó cùng một lúc, không chỉ$a_i$. Vì vậy, việc xây dựng lại phải tôn trọng điều kiện nhất quán toàn cục giữa các tiền tố. 

Các ràng buộc cho phép lên đến$10^6$tổng số phần tử trong các trường hợp thử nghiệm, điều này gợi ý rõ ràng về$O(n)$hoặc$O(n \log n)$mỗi giải pháp thử nghiệm. Bất kỳ cách tiếp cận nào liên quan đến việc kiểm tra tính nhất quán theo cặp hoặc quay lại các ứng cử viên sẽ là quá chậm. 

Một trường hợp lỗi tinh tế xuất hiện khi chuỗi gcd không tăng một cách đơn điệu. Vì tiền tố gcd chỉ có thể giữ nguyên hoặc giảm đi nên bất kỳ sự gia tăng nào cũng ngay lập tức khiến chuỗi không thể thực hiện được. Ví dụ, nếu$b = [6, 10]$, điều này không hợp lệ vì gcd của tiền tố lớn hơn không thể lớn hơn tiền tố gcd trước đó. 

Một sai lầm phổ biến khác là cho rằng việc thiết lập$a_i = b_i$luôn luôn hoạt động. Ví dụ, nếu$b = [6, 3]$, cài đặt$a = [6, 3]$mang lại tiền tố gcds$6, 3$, điều này không sao cả, nhưng nếu các ràng buộc sau này tạo ra mâu thuẫn thông qua cấu trúc chia hết, thì phép gán đơn giản có thể thất bại ở các chuỗi phức tạp hơn. 

## Phương pháp tiếp cận 

Việc tái thiết bằng vũ lực sẽ cố gắng đoán từng$a_i$và xác minh xem tiền tố kết quả gcd có khớp với tiền tố đã cho không$b$. Điều này liên quan đến việc tính toán lại các gcds nhiều lần cho từng mảng ứng cử viên, tính chi phí$O(n)$cho mỗi lần xác minh và vì có vô số lựa chọn ứng viên cho mỗi vị trí nên cách tiếp cận này thậm chí không được xác định rõ ràng là một không gian tìm kiếm hiệu quả. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ cố gắng xây dựng$a$tăng dần trong khi vẫn duy trì tất cả các ràng buộc tiền tố gcd. Ở bước$i$, chúng ta sẽ thử các giá trị cho$a_i$như vậy$\gcd(b_{i-1}, a_i) = b_i$, nhưng kiểm tra tất cả các bội số hợp lệ của$b_i$lên đến$10^9$dẫn đến việc tìm kiếm không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì điều trị$b_i$như một hạn chế đối với hành vi tiền tố không xác định, chúng tôi xây dựng một mảng ứng cử viên$a$_buộc_ tiền tố gcd tiến hóa phải khớp chính xác. Chúng tôi quan sát thấy rằng tiền tố gcd thay đổi từ$b_{i-1}$ĐẾN$b_i$, phần tử mới phải giới thiệu chính xác mức giảm gcd phù hợp và đủ để đảm bảo:$$\gcd(b_{i-1}, a_i) = b_i$$Điều này làm giảm vấn đề xây dựng từng phần tử một cách độc lập theo một ràng buộc gcd đơn giản. 

Chúng ta có thể thực thi điều này bằng cách chọn:$$a_i = b_i$$bất cứ khi nào$b_i$chia rẽ$b_{i-1}$, từ:$$\gcd(b_{i-1}, b_i) = b_i$$Tuy nhiên, chỉ điều này là không đủ để đảm bảo tính chính xác của quá trình phát triển tiền tố đầy đủ, bởi vì chúng ta cũng phải đảm bảo rằng các phần tử trước đó vẫn nhất quán với tất cả các giá trị gcd trong tương lai. 

Quan sát sâu hơn là khi tiền tố gcd là$b_i$, mọi tiền tố trước đó phải có gcd chính xác bằng gcd tương ứng của nó$b_j$. Điều này buộc một chuỗi phân chia:$$b_{i-1} \bmod b_i = 0$$phải giữ cho tất cả$i$. Nếu điều này không thành công ở bất cứ đâu, không có giải pháp nào tồn tại. 

Khi chuỗi hợp lệ, chúng ta có thể tạo một mảng hợp lệ đơn giản bằng cách đặt:$$a_i = b_i$$Điều này hoạt động vì tính toán gcd tiền tố trở thành:$$\gcd(b_1, b_2, \dots, b_i) = b_i$$miễn là mỗi bước là một sự chuyển đổi số chia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng$b$. Chúng tôi coi nó như chuỗi gcd tiền tố bắt buộc phải được một số mảng nhận ra chính xác$a$. 
2. Xác minh điều kiện cần thiết là mỗi tiền tố gcd không được tăng. Cụ thể, hãy kiểm tra điều đó cho mọi$i > 1$,$b_{i-1} \ge b_i$. Nếu điều này không thành công, hãy xuất -1 ngay lập tức vì giá trị gcd không thể tăng khi thêm nhiều phần tử hơn. 
3. Xác minh tính nhất quán của tính chia hết trong chuỗi bằng cách kiểm tra xem$b_{i-1} \bmod b_i = 0$cho tất cả$i > 1$. Điều này đảm bảo rằng mỗi bước có thể đạt được dưới dạng chuyển đổi gcd hợp lệ từ tiền tố trước đó. 
4. Nếu tất cả các bước kiểm tra đều đạt, hãy xây dựng câu trả lời bằng cách đặt$a_i = b_i$cho mọi vị trí. Lựa chọn này đảm bảo rằng mỗi tiền tố gcd khớp chính xác với chuỗi đã cho. 
5. Xuất mảng đã xây dựng. 

Lý do cấu trúc này được chọn là vì nó nhúng trực tiếp cấu trúc tiền tố mong muốn vào chính mảng, loại bỏ nhu cầu về bất kỳ điều chỉnh ẩn hoặc giá trị phụ trợ nào. 

### Tại sao nó hoạt động 

Ở mỗi bước$i$, chúng tôi duy trì rằng tiền tố gcd của mảng được xây dựng bằng$b_i$. Cảm ứng bắt đầu tầm thường tại$i = 1$, Ở đâu$a_1 = b_1$. Giả sử tiền tố gcd lên đến$i-1$bằng$b_{i-1}$, thêm$a_i = b_i$dẫn đến:$$\gcd(b_{i-1}, b_i) = b_i$$vì điều kiện chia hết. Điều này đảm bảo các chuyển đổi gcd tiền tố khớp chính xác với trình tự đã cho ở mỗi bước, do đó mảng được xây dựng là hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))

        ok = True
        for i in range(1, n):
            if b[i-1] < b[i] or b[i-1] % b[i] != 0:
                ok = False
                break

        if not ok:
            out.append("-1")
        else:
            out.append(" ".join(map(str, b)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai thực thi trực tiếp các điều kiện cấu trúc có được trong thuật toán. Vòng lặp đầu tiên kiểm tra cả hành vi không tăng đơn điệu và tính chia hết, đây là những ràng buộc duy nhất cần thiết để đảm bảo tính hợp lệ. Sau khi xác minh, sử dụng lại$b$vì mảng đầu ra là đủ. 

Một cạm bẫy triển khai phổ biến là quên kiểm tra tính chia hết và chỉ thực thi tính đơn điệu. Điều đó sẽ chấp nhận không chính xác các chuỗi như$[6, 4]$, không thể phát sinh từ bất kỳ mảng nào vì không có số nào có thể giảm gcd từ 6 xuống 4. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
b = [16, 8, 2, 2]
```| tôi | b[i-1] | b[i] | đơn điệu | chia hết | tiền tố hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | - | 16 | - | - | 16 | 
| 2 | 16 | 8 | vâng | vâng | 8 | 
| 3 | 8 | 2 | vâng | vâng | 2 | 
| 4 | 2 | 2 | vâng | vâng | 2 | 

Tất cả các bước kiểm tra đều đạt, vì vậy chúng tôi xuất ra$a = [16, 8, 2, 2]$. 

Dấu vết này cho thấy tiền tố gcd có thể giảm đều đặn như thế nào trong khi vẫn nhất quán thông qua khả năng chia hết. 

### Ví dụ 2 

đầu vào:```
n = 3
b = [30, 15, 5]
```| tôi | b[i-1] | b[i] | đơn điệu | chia hết | tiền tố hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | - | 30 | - | - | 30 | 
| 2 | 30 | 15 | vâng | vâng | 15 | 
| 3 | 15 | 5 | vâng | vâng | 5 | 

Chúng tôi xuất ra$a = [30, 15, 5]$. 

Điều này chứng tỏ một chuỗi chia số rõ ràng trong đó mỗi bước làm giảm gcd theo một hệ số nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được kiểm tra một lần về tính đơn điệu và tính chia hết | 
| Không gian | O(1) thêm | Mảng đầu ra là bộ lưu trữ bổ sung duy nhất ngoài đầu vào | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm là$10^6$, do đó quét tuyến tính cho mỗi trường hợp kiểm thử là đủ trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            b = list(map(int, input().split()))
            ok = True
            for i in range(1, n):
                if b[i-1] < b[i] or b[i-1] % b[i] != 0:
                    ok = False
                    break
            out.append("-1" if not ok else " ".join(map(str, b)))
        print("\n".join(out))

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    res = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return res

# provided samples (as consistent placeholders)
assert run("1\n4\n16 8 2 2\n") == "16 8 2 2"
assert run("1\n3\n30 15 5\n") == "30 15 5"

# custom cases
assert run("1\n2\n5 5\n") == "5 5", "all equal valid"
assert run("1\n2\n6 4\n") == "-1", "invalid non-divisible decrease"
assert run("1\n3\n10 5 6\n") == "-1", "increase in gcd invalid"
assert run("1\n1\n7\n") == "7", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n2\n5 5 | 5 5 | tất cả chuỗi gcd bằng nhau | 
| 1\n2\n6 4 | -1 | từ chối bước không chia hết | 
| 1\n3\n10 5 6 | -1 | gcd tăng trường hợp không hợp lệ | 
| 1\n1\n7 | 7 | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chuỗi giảm nhưng không chia hết. Đối với đầu vào$b = [12, 8]$, thuật toán kiểm tra$12 \bmod 8 \neq 0$, lập tức từ chối nó. Điều này phản ánh chính xác điều không thể xảy ra, vì không mảng nào có thể có tiền tố gcd 12 theo sau là 8. 

Một trường hợp cạnh khác là một chuỗi không đổi như$b = [7, 7, 7]$. Thuật toán xác minh tính đơn điệu và tính chia hết một cách tầm thường và đưa ra kết quả$a = [7, 7, 7]$, bảo toàn gcd trên tất cả các tiền tố. 

Trường hợp cạnh thứ ba có độ dài một chuỗi. Bất kỳ giá trị đơn lẻ nào cũng hợp lệ vì nó trực tiếp xác định mảng và không tồn tại ràng buộc nhất quán nào.
