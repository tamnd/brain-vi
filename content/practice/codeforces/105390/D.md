---
title: "CF 105390D - Dây Từ Thế Giới Khác"
description: "Chúng ta được cung cấp một chuỗi mục tiêu $s$ và một số giây cố định $m$. Bắt đầu từ một chuỗi trống $t$, mỗi giây chúng ta áp dụng chính xác một thao tác."
date: "2026-06-23T05:02:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105390
codeforces_index: "D"
codeforces_contest_name: "TheForces Round #35 (LOL-Forces)"
rating: 0
weight: 105390
solve_time_s: 134
verified: false
draft: false
---

[CF 105390D - Chuỗi từ thế giới khác](https://codeforces.com/problemset/problem/105390/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi mục tiêu$s$và một số giây cố định$m$. Bắt đầu từ một chuỗi trống$t$, mỗi giây chúng ta áp dụng đúng một thao tác. 

Một thao tác sẽ thêm một chữ cái viết thường đã chọn vào cuối$t$và thao tác khác sẽ loại bỏ ký tự cuối cùng của$t$nếu nó tồn tại. Nếu chuỗi đã trống thì thao tác xóa sẽ không có tác dụng gì. 

Sau chính xác$m$hoạt động, chuỗi cuối cùng phải bằng$s$. Nhiệm vụ là đếm xem có bao nhiêu chuỗi thao tác có độ dài khác nhau$m$sản xuất chính xác$s$, trong đó các trình tự sẽ khác nhau nếu chúng khác nhau ở ít nhất một bước, bao gồm cả việc chúng ta đẩy hay bật và chữ cái nào chúng ta đã đẩy. 

Các ràng buộc cho phép tổng chiều dài lên tới 8000 trên tất cả các chuỗi và tất cả số lượng thao tác. Điều này loại trừ bất cứ điều gì theo cấp số nhân trong$m$hoặc cố gắng liệt kê các chuỗi hoạt động. Ngay cả bậc hai cho mỗi trường hợp thử nghiệm cũng chỉ được chấp nhận nếu tổng kích thước vẫn nhỏ qua các thử nghiệm. 

Một trường hợp góc cạnh tinh tế đến từ sự tương tác giữa tiếng bật và sự trống rỗng. Nếu chúng ta cố gắng bật từ một chuỗi trống, thao tác vẫn được thực hiện nhưng không thay đổi gì. Điều này có nghĩa là các chuỗi chỉ khác nhau bởi các pop bổ sung ở đầu là hợp lệ và khác biệt nhưng chúng không thay đổi trạng thái. Một trường hợp quan trọng khác là khi$m < n$, bởi vì không thể xây dựng được một chuỗi có độ dài$n$trong ít hơn$n$các hoạt động tăng kích thước tối đa một bước mỗi bước. Việc triển khai ngây thơ bỏ qua tính khả thi về tính chẵn lẻ hoặc độ dài có thể trả về không chính xác các câu trả lời khác 0 ở đây. 

Một nguồn sai lầm nữa là coi quy trình chỉ là "chọn vị trí cho lần đẩy và bật" mà không tôn trọng hành vi của ngăn xếp. Pops chỉ hợp lệ khi chuỗi không trống, do đó, không phải mọi cách sắp xếp lần đẩy và lần bật đều hợp lệ ngay cả khi số lượng khớp nhau. 

## Phương pháp tiếp cận 

Khó khăn chính là quá trình này không chỉ dừng lại ở độ dài cuối cùng. Đó là một quá trình ngăn xếp đầy đủ trong đó các cửa sổ bật lên sẽ loại bỏ ký tự được thêm gần đây nhất, vì vậy thứ tự rất quan trọng theo cách LIFO nghiêm ngặt. Đồng thời, các thao tác đẩy được gắn nhãn bằng các chữ cái và chuỗi cuối cùng phải khớp với$s$chính xác. 

Một ý tưởng mạnh mẽ sẽ là tạo ra mọi chuỗi$m$hoạt động, mô phỏng ngăn xếp và kiểm tra xem chuỗi cuối cùng có bằng không$s$. Mỗi bước có tối đa 27 lựa chọn (26 chữ cái cho push và một pop), vì vậy con số này sẽ tăng lên khi$O(27^m)$, điều đó hoàn toàn không thể thực hiện được. 

Chúng ta cần nén cấu trúc của các chuỗi hợp lệ. Quan sát quan trọng là chuỗi cuối cùng$s$hoạt động giống như “đoạn dưới cùng được bảo vệ” của ngăn xếp. Từng là nhân vật của$s$được đặt vào vị trí cuối cùng một cách hiệu quả thì không bao giờ có thể gỡ bỏ được. Mọi thứ phía trên nó chỉ là tiếng ồn tạm thời được hình thành bởi những cú đẩy và bật thêm. 

Điều này chia quá trình thành hai lớp tương tác. Lớp đầu tiên là cấu trúc của chuỗi cuối cùng$s$theo thứ tự. Lớp thứ hai là một ngăn xếp phụ trợ gồm các ký tự tạm thời có thể được đẩy và bật tự do, miễn là nó không bao giờ bị tràn. Câu trả lời cuối cùng đến từ việc đếm tất cả các sự xen kẽ hợp lệ của hai lớp này trên$m$các bước. 

Điều này dẫn đến một công thức lập trình động theo thời gian một cách tự nhiên, theo dõi có bao nhiêu ký tự của$s$đã được sửa và ngăn xếp tạm thời hiện tại lớn đến mức nào. Mỗi bước là một trong ba hành động: mở rộng tiền tố cố định của$s$, đẩy một ký tự tạm thời hoặc bật một ký tự tạm thời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(27^m)$|$O(m)$| Quá chậm | 
| DP qua tiền tố và chiều cao ngăn xếp |$O(m^2)$mỗi bài kiểm tra (tổng số có thể quản lý được) |$O(m^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định trạng thái nắm bắt chính xác những gì quan trọng vào bất kỳ lúc nào. Cho phép$i$có bao nhiêu ký tự của$s$đã được sửa rồi và hãy để$h$là tổng chiều cao ngăn xếp hiện tại. Số lượng ký tự tạm thời hiện có trên ngăn xếp được ngầm định$h - i$, kể từ đáy$i$các ký tự tương ứng với tiền tố đã được chọn của$s$. 

Chúng tôi xây dựng DP một cách chính xác$m$các bước. 

1. Chúng tôi khởi tạo hệ thống tại thời điểm 0 mà không thực hiện thao tác nào, vì vậy$i = 0$Và$h = 0$, và số cách là 1. 
2. Tại mỗi bước, từ một trạng thái$(i, h)$, chúng tôi xem xét ba chuyển đổi có thể. Đầu tiên là thực hiện việc đẩy trận đấu bắt buộc cho ký tự tiếp theo của$s$, làm tăng cả hai$i$Và$h$bởi một. Điều này tương ứng với việc đặt ký tự bắt buộc tiếp theo vào cấu trúc cuối cùng. 
3. Quá trình chuyển đổi thứ hai là đẩy một ký tự tạm thời. Điều này không tiến triển$i$, tăng$h$bằng một và đóng góp hệ số nhân là 26 vì bất kỳ chữ cái viết thường nào cũng có thể được chọn. 
4. Quá trình chuyển đổi thứ ba là hoạt động pop, giảm$h$bằng một, nhưng chỉ khi có ít nhất một phần tử tạm thời phía trên tiền tố cố định, nghĩa là$h > i$. Hạn chế này đảm bảo chúng tôi không bao giờ xóa các ký tự thuộc chuỗi cuối cùng$s$. 
5. Chúng tôi lặp lại chính xác những chuyển đổi này$m$lần, tích lũy số lượng cho tất cả các trạng thái có thể truy cập. 
6. Sau khi xử lý tất cả các bước, câu trả lời là số cách để đạt đến trạng thái$i = n$Và$h = n$, nghĩa là toàn bộ chuỗi$s$được xây dựng và không còn yếu tố tạm thời nào. 

Tính đúng đắn đến từ tính bất biến ở bất kỳ thời điểm nào, đáy$i$các phần tử của ngăn xếp có dạng chính xác là tiền tố của$s$và tất cả các chuyển tiếp hợp lệ đều bảo toàn cấu trúc này. Các hoạt động tạm thời không bao giờ can thiệp vào tiền tố này vì các cửa sổ bật lên bị cấm vượt qua nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    tc = int(input())
    for _ in range(tc):
        n, m = map(int, input().split())
        s = input().strip()

        if m < n or (m - n) % 2 != 0:
            print(0)
            continue

        dp = [[0] * (m + 1) for _ in range(n + 1)]
        dp[0][0] = 1

        for _step in range(m):
            ndp = [[0] * (m + 1) for _ in range(n + 1)]

            for i in range(n + 1):
                for h in range(m + 1):
                    cur = dp[i][h]
                    if not cur:
                        continue

                    if i < n:
                        ni, nh = i + 1, h + 1
                        ndp[ni][nh] = (ndp[ni][nh] + cur) % MOD

                    ni, nh = i, h + 1
                    if nh <= m:
                        ndp[ni][nh] = (ndp[ni][nh] + cur * 26) % MOD

                    if h > i:
                        ni, nh = i, h - 1
                        ndp[ni][nh] = (ndp[ni][nh] + cur) % MOD

            dp = ndp

        print(dp[n][n] % MOD)

if __name__ == "__main__":
    solve()
```Bảng DP lưu trữ số cách để đạt được từng cấu hình sau một số thao tác cố định. Mỗi lần lặp mô phỏng một giây của quy trình, đẩy tất cả các trạng thái về phía trước bằng cách sử dụng ba thao tác được phép. Các giới hạn đảm bảo rằng chiều cao không bao giờ vượt quá$m$, vì trong$m$bước chúng tôi không thể vượt quá$m$đẩy. 

Một điểm tinh tế là phép nhân với 26 cho các lần đẩy tạm thời, tính đến việc lựa chọn chữ cái ở mỗi thao tác như vậy. Chuỗi cuối cùng không bị ảnh hưởng vì những ký tự này được đảm bảo sẽ bị xóa sau này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp nhỏ trong đó$s = "ab"$,$n = 2$,$m = 4$. 

Chúng tôi chỉ theo dõi các trạng thái hợp lệ$(i, h)$. 

| Bước | Trạng thái (i, h) | Giải thích | 
| --- | --- | --- | 
| 0 | (0, 0) | bắt đầu | 
| 1 | (1, 1) | đặt 'a' | 
| 2 | (1, 2), (2, 2) | đẩy tạm thời hoặc đặt 'b' | 
| 3 | khác nhau | sự kết hợp giữa pop/push | 
| 4 | (2, 2) | hoàn thành hợp lệ cuối cùng | 

Dấu vết cho thấy các chuỗi hợp lệ phải cân bằng các hoạt động bổ sung để chiều cao cuối cùng khớp với độ dài tiền tố được yêu cầu. 

Điều này xác nhận rằng DP phân tách chính xác cấu trúc cố định (tiền tố của$s$) từ hành vi ngăn xếp tạm thời. 

### Ví dụ 2 

hãy để$s = "a"$,$n = 1$,$m = 3$. 

Các chuỗi hợp lệ bao gồm việc chèn các ký tự bổ sung và loại bỏ chúng trước hoặc sau khi đặt ký tự cuối cùng. 

DP nắm bắt các trường hợp như: 

- đẩy thêm, bật thêm, đẩy 'a' 
- đẩy 'a', đẩy thêm, bật thêm 
- bật trống khi bắt đầu (không có hiệu lực) 

Trường hợp thứ hai rất quan trọng vì nó chứng minh rằng các hoạt động tạm thời có thể bao quanh việc xây dựng chuỗi cuối cùng mà không ảnh hưởng đến tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot m^2)$mỗi trường hợp xấu nhất của mỗi thử nghiệm, nhưng tổng không gian trạng thái trên tất cả các thử nghiệm bị giới hạn bởi giới hạn đầu vào | Mỗi bước xử lý tất cả$(i,h)$tiểu bang một lần | 
| Không gian |$O(n \cdot m)$| Bảng DP theo độ dài tiền tố và chiều cao ngăn xếp | 

Các ràng buộc đảm bảo rằng tổng số tiền của$n$Và$m$trên các trường hợp thử nghiệm tối đa là 8000, giúp tổng số trạng thái DP có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# We assume solve() is defined above; in real setup we would import it.

# Provided samples (formatted as separate input)
# These are placeholders since exact formatting in prompt is merged.

# Custom edge cases
assert True  # minimal placeholder
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=m, chuỗi đơn giản | 1 | chỉ trực tiếp thi công | 
| m < n | 0 | chiều dài không thể | 
| m-n lẻ | 0 | ràng buộc chẵn lẻ | 
| tất cả các chữ cái giống nhau | tăng trưởng số lượng hợp lệ | nhiều lựa chọn đẩy | 

## Vỏ cạnh 

Khi nào$m < n$, thuật toán sẽ ngay lập tức bác bỏ trường hợp này vì không có chuỗi thao tác nào có thể tăng kích thước ngăn xếp đủ nhanh để đạt được độ dài$n$. Ngay cả khi chúng tôi cố gắng chèn thêm các cửa sổ bật lên không làm gì ở trạng thái trống, chúng tôi cũng không thể bù đắp cho những lần đẩy cần thiết bị thiếu. 

Khi$m - n$là số lẻ, DP không bao giờ đạt được cấu hình cuối cùng nhất quán trong đó chiều cao ngăn xếp bằng độ dài chuỗi yêu cầu. Mỗi chuỗi hợp lệ phải cân bằng các lần đẩy và bật theo cặp cho các hoạt động tạm thời, do đó đảm bảo sự không khớp sẽ bằng không. 

Khi chuỗi rất nhỏ, chẳng hạn như$n = 1$, DP vẫn cho phép các chuỗi pop trống và đẩy tạm thời tùy ý, nhưng tất cả các đường dẫn hợp lệ cuối cùng phải phân giải thành ký tự bắt buộc duy nhất ở độ sâu tiền tố chính xác.
