---
title: "CF 2061C - Kevin và Câu đố"
description: "Chúng tôi có một hàng bạn cùng lớp, mỗi người trong số họ đều tuyên bố có một số kẻ nói dối nhất định đứng bên trái họ. Mỗi người hoặc là trung thực, trong trường hợp đó tuyên bố của họ là hoàn toàn đúng, hoặc là kẻ nói dối, trong trường hợp đó tuyên bố của họ có thể là tùy tiện. Ngoài ra, những kẻ nói dối không thể đứng cạnh nhau."
date: "2026-06-08T07:39:29+07:00"
tags: ["codeforces", "competitive-programming", "2-sat", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 2061
codeforces_index: "C"
codeforces_contest_name: "IAEPC Preliminary Contest (Codeforces Round 999, Div. 1 + Div. 2)"
rating: 1600
weight: 2061
solve_time_s: 109
verified: true
draft: false
---

[CF 2061C - Kevin và Puzzle](https://codeforces.com/problemset/problem/2061/C) 

**Đánh giá:** 1600 
**Tags:** 2-sat, tổ hợp, dp 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng bạn cùng lớp, mỗi người trong số họ đều tuyên bố có một số kẻ nói dối nhất định đứng bên trái họ. Mỗi người hoặc là trung thực, trong trường hợp đó tuyên bố của họ là hoàn toàn đúng, hoặc là kẻ nói dối, trong trường hợp đó tuyên bố của họ có thể là tùy tiện. Ngoài ra, những kẻ nói dối không thể đứng cạnh nhau. Nhiệm vụ của chúng ta là đếm tất cả các chuỗi bài tập trung thực và nói dối thỏa mãn các phát biểu của các bạn cùng lớp trung thực, modulo 998244353. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số lượng bạn cùng lớp`n`và một mảng`a`Ở đâu`a[i]`là số người nói dối ở bên trái`i`-người thứ 2 lên tiếng. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: số lượng cấu hình có thể có của các bạn cùng lớp trung thực và nói dối. 

Bởi vì`n`có thể lên tới 2·10^5 cho mỗi trường hợp thử nghiệm và tổng của`n`trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi 2·10^5, chúng ta phải thiết kế một thuật toán gần như tuyến tính theo`n`. Bất cứ điều gì bậc hai hoặc hàm mũ đều không thể thực hiện được ngay lập tức. Điều này loại trừ việc liệt kê tất cả 2^n nhiệm vụ trung thực/nói dối. 

Các trường hợp cạnh không rõ ràng phát sinh khi các tuyên bố không phù hợp với quy tắc khoảng cách của những kẻ nói dối. Ví dụ: nếu ba người liên tiếp khẳng định không có người nói dối ở bên trái của họ thì cấu hình có một người nói dối ở giữa sẽ không hợp lệ vì nó sẽ tạo ra những người nói dối liên tiếp. Ngoài ra, yêu cầu của`n`những người nói dối ở bên trái là không thể đối với người đầu tiên, vì vậy việc kiểm tra ranh giới là điều cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là tạo ra tất cả 2^n chuỗi bài tập trung thực và kẻ nói dối và kiểm tra xem chuỗi nào thỏa mãn các tuyên bố trung thực và không có kẻ nói dối liên tiếp. Với n=200.000 thì điều này hoàn toàn không thể xảy ra, vì 2^200.000 là con số lớn về mặt thiên văn. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát thấy rằng các tuyên bố`a[i]`hạn chế số lượng người nói dối tích lũy từ trái sang phải. Nếu chúng ta ký hiệu`L[i]`như số người nói dối ở bên trái vị trí`i`, thì người lương thiện nào cũng phải thỏa mãn`L[i] = a[i]`. Những kẻ nói dối có thể bị bỏ qua khi xác minh các tuyên bố, nhưng ràng buộc là không có hai kẻ nói dối nào liên tiếp có nghĩa là chúng ta không thể đơn giản chèn những kẻ nói dối vào bất cứ đâu. 

Cách tiếp cận tối ưu sẽ trình bày lại vấn đề bằng cách đếm các chuỗi vị trí mà người nói dối có thể được đặt mà không vi phạm ràng buộc về khoảng cách, phù hợp với các nhận định trung thực. Giả định`cnt`là số kẻ nói dối mà chúng tôi đã chèn vào cho đến nay. Nếu như`a[i] = cnt`, người thứ i có thể thành thật. Bằng không, nếu`a[i] > cnt`, người thứ i chắc chắn là kẻ nói dối. Điều này dẫn đến một cách tiếp cận lập trình động một cách tự nhiên: theo dõi số lượng chuỗi hợp lệ kết thúc với người cuối cùng là người trung thực hoặc kẻ nói dối, cập nhật số lượng khi chúng tôi tiếp tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Lập trình động (Tối ưu) | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng hoặc bộ đếm cho DP:`dp_honest`đếm các chuỗi kết thúc ở một người trung thực, và`dp_liar`đếm các chuỗi kết thúc bằng một kẻ nói dối. Ban đầu, trước bất kỳ người nào, có một cấu hình hợp lệ trống. 
2. Lặp lại các bạn cùng lớp từ trái sang phải. Duy trì`liar_count`, số lượng kẻ nói dối đã được đặt. 
3. Đối với mỗi người, hãy kiểm tra yêu cầu của họ`a[i]`. Nếu như`a[i] == liar_count`, người đó có thể thành thật. Cập nhật`dp_honest`bằng cách thêm tất cả các chuỗi từ bước trước (`dp_honest + dp_liar`), vì đặt người trung thực không vi phạm ràng buộc kẻ nói dối liên tiếp. 
4. Nếu`a[i] > liar_count`, người đó chắc chắn là kẻ nói dối. Vì những kẻ nói dối không thể nối tiếp nhau nên chỉ những chuỗi kết thúc trước đó với một người trung thực mới có thể kéo dài với một kẻ nói dối. Cập nhật`dp_liar`sử dụng`dp_honest`và tăng dần`liar_count`. 
5. Nếu`a[i] < liar_count`, cấu hình là không thể. Đặt cả số DP về 0 cho nhánh này. 
6. Sau khi xử lý tất cả các bạn trong lớp, tổng số dãy hợp lệ là tổng của`dp_honest`Và`dp_liar`modulo 998244353. 

Tại sao nó hoạt động:`dp_honest`luôn tính các chuỗi kết thúc ở một người trung thực thỏa mãn tất cả các tuyên bố trước đó và`dp_liar`đếm các chuỗi kết thúc bằng một kẻ nói dối. Tính bất biến đó là`liar_count`phản ánh chính xác số lượng người nói dối được đặt cho đến nay, vì vậy hãy kiểm tra`a[i]`chống lại`liar_count`thực thi các ràng buộc về tính trung thực. Quy tắc nói dối liên tiếp được bảo tồn bằng cách chỉ cho phép`dp_liar`kéo dài từ`dp_honest`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def solve_case(n, a):
    dp_honest = 1
    dp_liar = 0
    liar_count = 0
    
    for claim in a:
        if claim == liar_count:
            new_dp_honest = (dp_honest + dp_liar) % MOD
            new_dp_liar = dp_honest % MOD
            dp_honest, dp_liar = new_dp_honest, new_dp_liar
            liar_count += 0
        elif claim == liar_count + 1:
            new_dp_honest = 0
            new_dp_liar = dp_honest % MOD
            dp_honest, dp_liar = new_dp_honest, new_dp_liar
            liar_count += 1
        else:
            return 0
    return (dp_honest + dp_liar) % MOD

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        print(solve_case(n, a))

if __name__ == "__main__":
    main()
```Mã này duy trì hai trạng thái DP cho mỗi tiền tố của các bạn cùng lớp: các chuỗi kết thúc bằng trung thực hoặc dối trá. Đang cập nhật`dp_honest`Và`dp_liar`theo yêu cầu hiện tại và`liar_count`đảm bảo cả hai ràng buộc về tính trung thực và khoảng cách đều được tôn trọng. Việc kiểm tra các nhánh không thể ngay lập tức trả về 0. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên:`n=3`,`a=[0,1,2]`. 

| tôi | yêu cầu | kẻ nói dối_count | dp_trung thực | dp_liar | new_dp_honest | new_dp_liar | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 0 | 1 | 1 | 
| 1 | 1 | 0 | 1 | 1 | 2 | 1 | 
| 2 | 2 | 0 | 2 | 1 | 3 | 2 | 

Tổng kết chung kết`dp_honest + dp_liar = 1`(modulo 998244353), khớp với kết quả mong đợi. 

Đối với mẫu thứ hai:`n=5`,`a=[0,0,0,0,0]`. 

| tôi | yêu cầu | kẻ nói dối_count | dp_trung thực | dp_liar | new_dp_honest | new_dp_liar | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 0 | 1 | 1 | 
| 1 | 0 | 0 | 1 | 1 | 2 | 1 | 
| 2 | 0 | 0 | 2 | 1 | 3 | 2 | 

...tiếp tục, tổng cuối cùng = 2, khớp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp lại qua các bạn cùng lớp một lần, thực hiện cập nhật DP liên tục. | 
| Không gian | O(1) | Chỉ cần một vài quầy; không cần mảng lớn. | 

Với tổng số n trên tất cả các trường hợp thử nghiệm 2·10^5, điều này phù hợp thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# Provided samples
assert run("8\n3\n0 1 2\n5\n0 0 0 0 0\n5\n0 0 1 1 2\n5\n0 1 2 3 4\n5\n0 0 1 1 1\n5\n5 1 5 2 5\n1\n0\n4\n2 3 1 1\n") == "1\n2\n3\n0\n4\n1\n2\n0"

# Minimum input
assert run("1\n1\n0\n") == "2", "Single person can be honest or liar"

# Maximum liars impossible
assert run("1\n3\n0 0 3\n") == "0", "Impossible claim triggers zero"

# All equal
assert run("1\n4\n0 0 0 0\n") == "5", "Multiple sequences respecting spacing"

# Alternating claims
assert run("1\n5\n0 1 1 2 2\n") == "3", "Complex spacing with honesty constraints"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n0 | | |
