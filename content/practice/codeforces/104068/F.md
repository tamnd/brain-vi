---
title: "CF 104068F - Toxel \u4e0e Làng: Vòng tròn"
description: "Chúng ta có một tập hợp các đỉnh được gắn nhãn $n$ mà chúng phải trở thành lá của cây. Chúng ta được phép thêm các đỉnh bổ sung, nhưng các đỉnh bổ sung đó không thể phân biệt được với nhau và mọi đỉnh được thêm vào như vậy phải có bậc chính xác là 3 trong cây cuối cùng."
date: "2026-07-02T03:04:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104068
codeforces_index: "F"
codeforces_contest_name: "The 17-th Beihang University Collegiate Programming Contest (BCPC 2022) - Preliminary"
rating: 0
weight: 104068
solve_time_s: 53
verified: true
draft: false
---

[CF 104068F - Toxel \u4e0e Làng: Vòng tròn đất liền](https://codeforces.com/problemset/problem/104068/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ$n$các đỉnh được dán nhãn phải trở thành lá của cây. Chúng ta được phép thêm các đỉnh bổ sung, nhưng các đỉnh bổ sung đó không thể phân biệt được với nhau và mọi đỉnh được thêm vào như vậy phải có bậc chính xác là 3 trong cây cuối cùng. Mỗi đỉnh được gắn nhãn ban đầu phải kết thúc bằng chính xác bậc 1, nghĩa là mọi đỉnh được gắn nhãn là một chiếc lá. 

Vì vậy, cấu trúc chúng ta đang đếm là một cây trong đó tất cả các đỉnh được dán nhãn đều là lá và mỗi đỉnh bên trong có bậc 3. Hai cây được coi là khác nhau nếu cấu trúc kề của chúng trên các lá được dán nhãn khác nhau. Các đỉnh bên trong không được gắn nhãn nên việc đổi tên chúng không tạo ra cấu hình mới. 

Đầu vào chứa nhiều trường hợp kiểm thử, mỗi trường hợp cho một giá trị là$n$, và chúng ta phải xuất ra số lượng cây hợp lệ theo modulo 998244353. 

Các ràng buộc ngụ ý rằng chúng ta không thể làm bất cứ điều gì thậm chí là tuyến tính cho mỗi trường hợp thử nghiệm. Với tối đa$10^6$truy vấn và$n$cũng lên đến$10^6$, mọi phép tính lại DFS, DP hoặc giai thừa theo từng thử nghiệm sẽ quá chậm trừ khi được tính toán trước đầy đủ một lần. Giải pháp phải giảm từng truy vấn về thời gian không đổi sau một lần xử lý trước. 

Một cách tiếp cận đơn giản sẽ cố gắng liệt kê các cấu trúc liên kết cây kết nối các lá được dán nhãn thông qua các nút bên trong cấp 3. Ngay cả đối với nhỏ$n$, số lượng hình dạng cây không được gắn nhãn sẽ tăng theo cấp số nhân. Ví dụ, với$n=6$, các cách khác nhau để nhóm các lá thông qua các nút bên trong đã tạo ra nhiều cấu trúc tổ hợp và việc tạo trực tiếp là không khả thi. 

Một kiểu thất bại phổ biến là cố gắng “xây dựng cây tăng dần” bằng cách gắn từng lá một và đếm các lựa chọn cục bộ. Điều này bị tính quá nhiều vì nhiều trình tự xây dựng dẫn đến cùng một cấu trúc bên trong không được gắn nhãn cuối cùng. 

## Phương pháp tiếp cận 

Quan điểm brute-force là nghĩ đến việc xây dựng một cái cây có lá chính xác là các đỉnh được dán nhãn. Mỗi nút bên trong phải có cấp 3, vì vậy mỗi nút bên trong chia cấu trúc còn lại thành ba phần. Người ta có thể tưởng tượng việc phân chia đệ quy các lá được dán nhãn thành các nhóm được gắn thông qua các nút bên trong. Điều này nhanh chóng trở thành một bài toán đếm đối với tất cả các phân rã bậc ba của một tập hợp có nhãn, và mỗi phân tách tương ứng với nhiều sắp xếp bên trong không được gắn nhãn tương đương. 

Quan sát quan trọng là cấu trúc này không phải là tùy ý. Bất kỳ cây nào mà tất cả các nút bên trong đều có cấp độ 3 và tất cả các đỉnh được dán nhãn đều là lá chính xác là cây phát sinh gen nhị phân đầy đủ không có rễ. Một kết quả cổ điển trong tổ hợp cho biết số lượng cây như vậy trên$n$lá được dán nhãn là$$(2n - 5)!!$$là tích của tất cả các số nguyên lẻ từ 1 đến$2n - 5$. 

Công thức này cũng có thể được suy ra bằng quy nạp. Nếu chúng ta đã biết số lượng cây cho$n-1$lá, việc thêm một lá có nhãn mới tương ứng với việc chia nhỏ một cạnh hiện có và gắn lá mới. Ở bước$n$, có chính xác$2n-5$các cạnh trong bất kỳ cây hợp lệ nào, mang lại phép truy hồi nhân. 

Do đó, chúng tôi tránh hoàn toàn mọi phép liệt kê cấu trúc và thay vào đó tính toán một chuỗi sản phẩm đơn giản. 

Lực lượng vũ phu không thành công vì nó cố gắng khám phá nhiều hình dạng cây theo cấp số nhân, trong khi chế độ xem chính xác thu gọn toàn bộ cấu trúc thành một sản phẩm dạng đóng duy nhất qua các lần chèn tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| công thức nhân$(2n-5)!!$|$O(n + T)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Việc tính toán giảm xuống còn tính toán trước tất cả các giá trị của$(2n-5)!!$lên đến mức tối đa$n$trong đầu vào. 

1. Quan sát rằng trường hợp cơ sở là$n = 3$, nơi có chính xác một cây hợp lệ. Điều này phù hợp với thực tế rằng$(2\cdot 3 - 5)!! = 1!! = 1$. 
2. Duy trì một mảng`dp[n]`Ở đâu`dp[n]`lưu trữ số lượng cây hợp lệ cho$n$lá được dán nhãn. 
3. Sử dụng phép truy hồi rút ra từ việc chèn cạnh: khi tăng số lá từ$n-1$ĐẾN$n$, số vị trí đính kèm có sẵn là$2n - 5$. Điều này mang lại$$dp[n] = dp[n-1] \cdot (2n - 5)$$4. Tính toán trước`dp[n]`lặp đi lặp lại từ$n=3$đến giá trị đầu vào tối đa. 
5. Đối với mỗi truy vấn, xuất trực tiếp`dp[n]`. 

Việc tính toán hoàn toàn mang tính nhân, vì vậy tất cả các giá trị được xây dựng trong một lần quét tuyến tính. 

Tính đúng đắn dựa trên tính bất biến mà mọi cây hợp lệ với$n$lá được dán nhãn có chính xác$2n-5$các cạnh nơi một lá mới có thể được chèn vào trong khi vẫn giữ nguyên các ràng buộc về mức độ. Mỗi lần chèn có thể đảo ngược và tạo ra một cây lớn hơn duy nhất, do đó phép nhân sẽ tính mỗi phần mở rộng chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def main():
    t = int(input())
    ns = [int(input()) for _ in range(t)]
    max_n = max(ns)

    dp = [0] * (max_n + 1)

    if max_n >= 3:
        dp[3] = 1
        for n in range(4, max_n + 1):
            dp[n] = dp[n - 1] * (2 * n - 5) % MOD

    out = []
    for n in ns:
        out.append(str(dp[n]))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Mảng DP được xây dựng một lần với số truy vấn tối đa$n$. Mỗi quá trình chuyển đổi nhân với một thuật ngữ tuyến tính lẻ xuất phát từ số cạnh có sẵn trong cây tương ứng. Các truy vấn được trả lời theo thời gian liên tục bằng cách tra cứu. 

Một điểm tinh tế là khởi tạo tại$n=3$. Bắt đầu từ cơ sở này sẽ tránh được việc xử lý các chỉ số âm không hợp lệ trong công thức. Sự lặp lại chỉ có hiệu lực từ$n \ge 4$. 

## Ví dụ đã hoạt động 

Hãy xem xét các giá trị nhỏ để xem trình tự diễn ra như thế nào. 

Vì$n=3$, chúng tôi đặt: 

| n | dp[n] | chuyển tiếp | 
| --- | --- | --- | 
| 3 | 1 | căn cứ | 

Vì$n=4$: 

| n | dp[n] | chuyển tiếp | 
| --- | --- | --- | 
| 3 | 1 | căn cứ | 
| 4 | 1 × 3 = 3 | nhân với$2·4-5=3$| 

Vì$n=5$: 

| n | dp[n] | chuyển tiếp | 
| --- | --- | --- | 
| 3 | 1 | căn cứ | 
| 4 | 3 | bước trước | 
| 5 | 3 × 5 = 15 | nhân với$2·5-5=5$| 

Điều này phù hợp với sự tăng trưởng dự kiến ​​của cấu trúc liên kết cây khi lá tăng lên. Mỗi bước giới thiệu số lượng vị trí đính kèm ngày càng tăng, phản ánh cấu trúc tổ hợp mở rộng của cây khối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n_{\max} + T)$| Một tính toán trước tuyến tính cộng với các truy vấn thời gian không đổi | 
| Không gian |$O(n_{\max})$| Lưu trữ giá trị DP lên đến mức tối đa$n$| 

Những ràng buộc cho phép$n_{\max} \le 10^6$Và$T \le 10^6$, do đó, một lần truyền qua mảng DP là khả thi trong giới hạn thời gian và tất cả các truy vấn đều được trả lời bằng cách tra cứu trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    MOD = 998244353

    t = int(input())
    ns = [int(input()) for _ in range(t)]
    max_n = max(ns)

    dp = [0] * (max_n + 1)
    if max_n >= 3:
        dp[3] = 1
        for n in range(4, max_n + 1):
            dp[n] = dp[n - 1] * (2 * n - 5) % MOD

    return "\n".join(str(dp[n]) for n in ns)

# sample-like checks
assert run("1\n3\n") == "1"
assert run("1\n4\n") == "3"

# additional cases
assert run("3\n3\n4\n5\n") == "1\n3\n15"
assert run("2\n6\n7\n") == str((15 * 7) % 998244353) + "\n" + str((15 * 7 * 9) % 998244353)
assert run("1\n10\n") == str(__import__("math").prod(range(1, 2*10-4, 2)) % 998244353)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trình tự 3,4,5 | 1,3,15 | tính đúng đắn của sự tái phát | 
| 6,7 | giá trị tính toán | tăng trưởng nhất quán | 
| 10 | kiểm tra giai thừa lẻ | căn chỉnh công thức | 

## Vỏ cạnh 

cho$n=3$, cây không có cấu trúc bên trong và câu trả lời phải là 1. Thuật toán đặt rõ ràng trường hợp cơ sở này, do đó không có phép lặp nào được áp dụng sai ở các chỉ số nhỏ hơn. 

Đối với lớn$n$, sản phẩm phát triển nhanh chóng nhưng vẫn có thể quản lý được theo số học modulo. Việc triển khai đảm bảo phép nhân luôn được giảm theo modulo 998244353, ngăn ngừa tràn và giữ cho các giá trị bị giới hạn. 

Vì$n=4$, có đúng 3 cây hợp lệ. Đây là trường hợp không tầm thường đầu tiên trong đó sự tái diễn kích hoạt và nó xác minh rằng yếu tố$2n-5$đếm chính xác các vị trí đính kèm có sẵn.
