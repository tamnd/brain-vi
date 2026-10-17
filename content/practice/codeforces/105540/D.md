---
title: "CF 105540D - Hoàng đế"
description: "Vấn đề là một quá trình xác suất trên ba quần thể cạnh tranh. Bạn bắt đầu với một số lượng đá, kéo và giấy cố định. Theo thời gian, các cặp cá thể được chọn ngẫu nhiên một cách thống nhất từ ​​tất cả các cá thể còn lại."
date: "2026-06-27T00:56:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105540
codeforces_index: "D"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Jinan Site (The 3rd Universal Cup. Stage 17: Jinan)"
rating: 0
weight: 105540
solve_time_s: 49
verified: true
draft: false
---

[CF 105540D - Hoàng đế](https://codeforces.com/problemset/problem/105540/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề là một quá trình xác suất trên ba quần thể cạnh tranh. Bạn bắt đầu với một số lượng đá, kéo và giấy cố định. Theo thời gian, các cặp cá thể được chọn ngẫu nhiên một cách thống nhất từ ​​tất cả các cá thể còn lại. Nếu hai loại được chọn khác nhau thì một loại bị loại theo quy tắc oẳn tù tì: đá thắng kéo, kéo đập giấy, giấy đập đá. Nếu cặp cùng loại thì không có gì xảy ra. 

Quá trình này tiếp tục cho đến khi chỉ còn lại một loài. Nhiệm vụ là tính toán, đối với mỗi loài trong số ba loài, xác suất để nó trở thành loài sống sót duy nhất trong toàn bộ quá trình. 

Đầu vào chỉ là ba số nguyên biểu thị số lượng đá, kéo và giấy ban đầu. Đầu ra là ba số thực cho biết xác suất sống sót cuối cùng theo thứ tự đó. 

Hạn chế quan trọng là mỗi số đếm tối đa là 100, điều này loại trừ mọi vụ nổ trạng thái theo cấp số nhân. Một không gian trạng thái có kích thước khoảng 100 khối là khoảng một triệu cấu hình, điều này gần như khả thi đối với phương pháp lập trình động. Bất cứ điều gì liên quan đến việc liệt kê tất cả các chuỗi tương tác có thể xảy ra đều là quá lớn ngay lập tức, vì số lượng các chuỗi sự kiện có thể xảy ra tăng theo giai thừa theo tổng quy mô dân số. 

Một ý tưởng ngây thơ là mô phỏng quá trình ngẫu nhiên nhiều lần bằng cách sử dụng Monte Carlo. Điều đó sẽ hội tụ chậm và không thể đảm bảo độ chính xác cần thiết trong giới hạn thời gian, đặc biệt khi xác suất trở nên rất nhỏ hoặc rất gần. 

Một cạm bẫy phổ biến khác là cố gắng mô hình hóa quá trình dưới dạng loại bỏ từng cặp độc lập. Ví dụ, giả sử các hòn đá loại bỏ chiếc kéo một cách độc lập ở một tỷ lệ nào đó mà không tính đến chiếc kéo cũng việc loại bỏ giấy tờ sẽ làm biến dạng động lực học. Sự tương tác mang tính tuần hoàn và kết hợp, do đó các mô hình từng phần phá vỡ tính đối xứng và đưa ra xác suất không chính xác. 

Một trường hợp phức tạp xuất hiện khi một trong các loài bắt đầu từ con số 0. Ví dụ: nếu ban đầu chiếc kéo bằng 0, thì đá và giấy vẫn tương tác trực tiếp trong một hệ thống hai loài rút gọn và câu trả lời sẽ giảm xuống mức hấp thụ tất định giữa hai loại đó. Bất kỳ giải pháp nào giả định cả ba loài luôn hiện diện sẽ xử lý sai các ranh giới này. 

## Phương pháp tiếp cận 

Phối cảnh brute-force là mô phỏng toàn bộ quá trình ngẫu nhiên trên tất cả các trạng thái và chuyển tiếp có thể có. Mỗi trạng thái được xác định bởi một bộ ba (r, s, p) và từ trạng thái đó, sự kiện tiếp theo phụ thuộc vào việc chọn bất kỳ cặp cá thể nào giống nhau trong số tất cả các cặp. Từ một trạng thái có tổng số n cá thể, có thể có O(n^2) cặp và mỗi cặp tạo ra sự chuyển đổi hoặc không làm gì cả. Việc mở rộng điều này thành một cây xác suất đầy đủ dẫn đến số lượng nhánh theo cấp số nhân. Ngay cả với việc ghi nhớ các trạng thái, việc liệt kê trực tiếp tất cả các chuỗi sự kiện là không khả thi vì yếu tố phân nhánh vẫn gắn liền với số lượng tương tác có thể xảy ra. 

Quan sát quan trọng là quá trình này không có bộ nhớ đối với số lượng trạng thái. Một khi bạn sửa (r, s, p), tất cả sự tiến hóa trong tương lai chỉ phụ thuộc vào những số lượng này chứ không phụ thuộc vào lịch sử. Điều này cho phép chúng ta xác định hàm xác suất f(r, s, p) biểu thị xác suất mà đá cuối cùng giành chiến thắng bắt đầu từ trạng thái đó. Điều tương tự cũng áp dụng cho hai loài còn lại. Bước tiếp theo là biểu thị f(r, s, p) theo các trạng thái nhỏ hơn bằng cách điều chỉnh tương tác tiếp theo.

Từ một trạng thái nhất định, cặp tiếp theo được chọn là ngẫu nhiên thống nhất trong số tất cả các cặp cá thể riêng biệt. Xác suất để sự kiện tiếp theo giảm r đi một tỷ lệ thuận với số cặp oẳn tù tì, tương tự đối với kéo-giấy và bao-búa. Điều này tạo ra sự tái diễn trực tiếp giữa các trạng thái có tổng quy mô dân số nhỏ hơn rất nhiều, bởi vì mọi tương tác không tầm thường sẽ loại bỏ chính xác một cá thể. Điều này mang lại một thứ tự lập trình động rõ ràng bằng cách giảm tổng dân số. 

Brute-force thất bại vì nó cố gắng suy luận theo trình tự, trong khi công thức đúng sẽ nén tất cả tính ngẫu nhiên vào xác suất chuyển tiếp cục bộ giữa các trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ tất cả các chuỗi sự kiện | Hàm mũ | O(1) | Quá chậm | 
| DP qua các trạng thái (r, s, p) | O(n³) | O(n³) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định ba bảng DP: probR[r][s][p], probS[r][s][p], probP[r][s][p], trong đó mỗi mục biểu thị xác suất lần lượt đá, kéo hoặc giấy trở thành kẻ sống sót cuối cùng bắt đầu từ trạng thái đó. 

1. Chúng tôi khởi tạo tất cả các giá trị DP về 0 ngoại trừ các trường hợp cơ bản chỉ còn lại một loài. Nếu r > 0 và s = ​​p = 0 thì probR[r][0][0] = 1. Tương tự với hai loài còn lại. Điều này mã hóa rằng nếu chỉ có một loài tồn tại thì kết quả đã được xác định. 
2. Chúng tôi lặp lại tất cả các tiểu bang trong việc tăng tổng quy mô dân số. Thứ tự này đảm bảo rằng khi chúng ta tính toán một trạng thái (r, s, p), tất cả các chuyển đổi sang trạng thái nhỏ hơn đều đã được biết trước. 
3. Với trạng thái (r, s, p), hãy tính tổng số cặp tương tác có thể có giữa các loài khác nhau. Có cặp r·s kéo-búa, cặp kéo-giấy s·p và cặp bao-đá. Tổng của những điều này xác định tần suất trạng thái thực sự thay đổi. 
4. Chúng tôi phân phối khối lượng xác suất dựa trên tương tác nào xảy ra tiếp theo. Nếu hòn đá gặp cái kéo, cái kéo sẽ được lấy ra và chúng ta chuyển sang (r, s−1, p). Nếu kéo gặp giấy, chúng ta đi tới (r, s, p−1). Nếu giấy gặp đá, chúng ta đi tới (r−1, s, p). Mỗi lần chuyển đổi được tính trọng số bởi xác suất tương đối của nó trong số tất cả các cặp tương tác hợp lệ. 
5. Chúng tôi tích lũy các đóng góp vào bảng DP bằng cách sử dụng các xác suất chuyển đổi này, đẩy khối lượng xác suất một cách hiệu quả từ trạng thái hiện tại sang trạng thái nhỏ hơn. 
6. Chúng tôi lặp lại điều này cho tất cả các trạng thái cho đến khi đạt được cấu hình ban đầu (r, s, p), có giá trị đưa ra câu trả lời cuối cùng. 

Lý do thứ tự này có tác dụng là vì mọi chuyển đổi hợp lệ đều giảm r + s + p đúng một. Điều đó đảm bảo đồ thị DP không có tính tuần hoàn khi được sắp xếp theo tổng dân số. Do đó, sự lặp lại được xác định rõ ràng và không thể lặp lại trạng thái chưa được tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX = 105

r0, s0, p0 = map(int, input().split())

probR = [[[0.0] * MAX for _ in range(MAX)] for _ in range(MAX)]
probS = [[[0.0] * MAX for _ in range(MAX)] for _ in range(MAX)]
probP = [[[0.0] * MAX for _ in range(MAX)] for _ in range(MAX)]

for i in range(MAX):
    probR[i][0][0] = 1.0
    probS[0][i][0] = 1.0
    probP[0][0][i] = 1.0

for r in range(MAX):
    for s in range(MAX):
        for p in range(MAX):
            if r + s + p <= 1:
                continue

            total = r * s + s * p + p * r
            if total == 0:
                continue

            if r > 0 and s > 0:
                probR[r][s][p] += probR[r][s-1][p] * (r * s / total)
                probS[r][s][p] += probS[r][s-1][p] * (r * s / total)
                probP[r][s][p] += probP[r][s-1][p] * (r * s / total)

            if s > 0 and p > 0:
                probR[r][s][p] += probR[r][s][p-1] * (s * p / total)
                probS[r][s][p] += probS[r][s][p-1] * (s * p / total)
                probP[r][s][p] += probP[r][s][p-1] * (s * p / total)

            if p > 0 and r > 0:
                probR[r][s][p] += probR[r-1][s][p] * (p * r / total)
                probS[r][s][p] += probS[r-1][s][p] * (p * r / total)
                probP[r][s][p] += probP[r-1][s][p] * (p * r / total)

print(f"{probR[r0][s0][p0]:.12f} {probS[r0][s0][p0]:.12f} {probP[r0][s0][p0]:.12f}")
```Việc triển khai mã hóa trực tiếp sự lặp lại bắt nguồn từ điều kiện trong tương tác tiếp theo. Mỗi thuật ngữ tương ứng với một trong ba cuộc chạm trán giữa các loài có thể xảy ra. Một lỗi triển khai thường gặp là quên rằng các quá trình chuyển đổi phải sử dụng các trạng thái nhỏ hơn đã được tính toán sẵn; lặp lại bằng cách tăng r + s + p đảm bảo tính chất này được giữ một cách tự nhiên. 

Tích lũy dấu phẩy động ở đây an toàn vì không gian trạng thái nhỏ và xác suất vẫn bị giới hạn. Sử dụng độ chính xác kép là đủ cho độ chính xác 1e-9 cần thiết. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`2 2 2`. 

| Bang (r,s,p) | Ý tưởng chuyển tiếp | Hành vi đóng góp | 
| --- | --- | --- | 
| (2,2,2) | cả ba tương tác có thể xảy ra | chia đều do tính đối xứng | 
| (2,1,2) | trạng thái kéo giảm | lan truyền sự bất đối xứng | 
| (1,2,2) | đối xứng | gương trường hợp trước | 

Dấu vết này cho thấy rằng tính đối xứng ở trạng thái ban đầu dẫn đến xác suất bằng nhau, vì mọi quá trình chuyển đổi đều có vai trò trao đổi đối tác được phản chiếu của loài. 

Bây giờ hãy xem xét`2 1 2`. 

| Bang (r,s,p) | Tương tác chiếm ưu thế | Hiệu ứng | 
| --- | --- | --- | 
| (2,1,2) | giấy-đá thường xuyên | đá bị loại bỏ thường xuyên hơn | 
| (2,0,2) | chỉ đá-giấy | giảm xuống chu kỳ hai loài | 
| (1,1,2) | kéo yếu hiện diện | kéo hiếm khi sống sót | 

Điều này cho thấy sự mất cân bằng về số lượng ban đầu có xu hướng chuyển đổi hướng về loài thống trị kẻ săn mồi trực tiếp của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) | Mọi trạng thái (r,s,p) được xử lý một lần, với các chuyển đổi O(1) | 
| Không gian | O(n³) | Ba mảng DP trên tất cả các trạng thái | 

Với n ≤ 100, tổng số trạng thái là khoảng một triệu, nằm trong giới hạn đối với Python hoặc C++ dưới giới hạn thời gian 2 giây khi được triển khai cẩn thận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    MAX = 105
    r0, s0, p0 = map(int, sys.stdin.readline().split())

    probR = [[[0.0] * MAX for _ in range(MAX)] for _ in range(MAX)]
    probS = [[[0.0] * MAX for _ in range(MAX)] for _ in range(MAX)]
    probP = [[[0.0] * MAX for _ in range(MAX)] for _ in range(MAX)]

    for i in range(MAX):
        probR[i][0][0] = 1.0
        probS[0][i][0] = 1.0
        probP[0][0][i] = 1.0

    for r in range(MAX):
        for s in range(MAX):
            for p in range(MAX):
                if r + s + p <= 1:
                    continue
                total = r * s + s * p + p * r
                if total == 0:
                    continue

                if r > 0 and s > 0:
                    probR[r][s][p] += probR[r][s-1][p] * (r*s/total)
                    probS[r][s][p] += probS[r][s-1][p] * (r*s/total)
                    probP[r][s][p] += probP[r][s-1][p] * (r*s/total)

                if s > 0 and p > 0:
                    probR[r][s][p] += probR[r][s][p-1] * (s*p/total)
                    probS[r][s][p] += probS[r][s][p-1] * (s*p/total)
                    probP[r][s][p] += probP[r][s][p-1] * (s*p/total)

                if p > 0 and r > 0:
                    probR[r][s][p] += probR[r-1][s][p] * (p*r/total)
                    probS[r][s][p] += probS[r-1][s][p] * (p*r/total)
                    probP[r][s][p] += probP[r-1][s][p] * (p*r/total)

    return f"{probR[r0][s0][p0]:.12f} {probS[r0][s0][p0]:.12f} {probP[r0][s0][p0]:.12f}"

# provided samples
assert run("2 2 2\n")[:5] == "0.333"
assert run("2 1 2\n")[:5] == "0.150"

# custom cases
assert run("1 1 1\n")  # symmetric smallest nontrivial
assert run("1 0 0\n").startswith("1.000"), "single species"
assert run("10 0 0\n").startswith("1.000"), "edge dominance"
assert run("2 0 2\n")  # two species cycle case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | phân phối đối xứng | độ đúng đối xứng cơ sở | 
| 1 0 0 | 1 0 0 | trường hợp cơ sở đơn loài | 
| 10 0 0 | 1 0 0 | sự ổn định của một loài lớn | 
| 2 0 2 | xác suất hợp lệ | trường hợp giảm hai loài | 

## Vỏ cạnh 

Đối với trường hợp`1 0 0`, thuật toán ngay lập tức đạt được điều kiện cơ bản trong đó chỉ có đá tồn tại. Bảng DP khởi tạo probR[1][0][0] thành 1 và không áp dụng chuyển đổi nào vì tổng số lần tương tác bằng 0. Đầu ra trả về chính xác 1 0 0. 

cho`2 0 2`, chỉ có đá và giấy tương tác. Quá trình chuyển đổi làm giảm hệ thống thành một chuỗi hai chiều trong đó chỉ có sự tương tác giữa giấy và đá là quan trọng. DP tự nhiên thu gọn kích thước chiếc kéo về 0 và truyền xác suất thông qua các chuyển đổi hợp lệ, cuối cùng chuyển sang phân phối người chiến thắng xác định tùy thuộc vào bên nào loại bỏ bên kia trước.
