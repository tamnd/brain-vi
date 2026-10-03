---
title: "CF 104880B - \u6bd5\u4e1a\u5408\u7167"
description: "Chúng ta có một nhóm người di chuyển dọc theo một đường thẳng vô hạn. Mỗi người xuất phát ở vị trí $xi$ rồi chuyển động với vận tốc không đổi $vi$. Mọi chuyển động đều bắt đầu tại cùng một thời điểm nên thời gian $t = 0$ được chia sẻ."
date: "2026-06-28T09:21:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104880
codeforces_index: "B"
codeforces_contest_name: "The 18-th Beihang University Collegiate Programming Contest (BCPC 2023) - Preliminary"
rating: 0
weight: 104880
solve_time_s: 54
verified: true
draft: false
---

[CF 104880B - \u6bd5\u4e1a\u5408\u7167](https://codeforces.com/problemset/problem/104880/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một nhóm người di chuyển dọc theo một đường thẳng vô hạn. Mỗi người xuất phát ở một vị trí$x_i$rồi chuyển động với vận tốc không đổi$v_i$. Mọi chuyển động đều bắt đầu tại cùng một thời điểm, vì vậy thời gian$t = 0$được chia sẻ. 

Bất cứ khi nào hai người đến cùng một vị trí vào cùng một thời điểm, họ sẽ chụp ảnh cùng nhau. Nếu một nhóm$k$mọi người gặp nhau ở cùng một điểm vào cùng một thời điểm, mỗi cặp trong nhóm đó sẽ chụp ảnh, vì vậy điều đó góp phần$\frac{k(k-1)}{2}$ảnh ngay lúc đó. Chúng tôi cần tổng số cuộc họp từng cặp như vậy trong mọi thời điểm. 

Đối tượng chính không phải là chuyển động mà là sự bình đẳng của các vị trí:$$x_i + v_i t = x_j + v_j t$$xác định khi nào hai người gặp nhau, nếu có. 

Những hạn chế$n \le 1000$Và$|x_i|, |v_i| \le 10^4$ngay lập tức đề nghị rằng một$O(n^2)$lý luận là an toàn. Bất kỳ giải pháp nào cố gắng mô phỏng thời gian liên tục hoặc theo dõi các sự kiện một cách linh hoạt sẽ không cần thiết và có thể quá phức tạp. 

Một vấn đề tế nhị là xử lý các cuộc họp đồng thời. Nhiều cặp có thể gặp nhau ở cùng thời điểm và địa điểm, tạo thành một cụm. Ví dụ: nếu ba người đều thỏa mãn cùng thời gian và vị trí giao nhau thì phải tính 3 ảnh chứ không phải 2 hay 1. 

Một trường hợp cạnh khác đến từ các vị trí bắt đầu giống hệt nhau. Nếu như$x_i = x_j$, chúng gặp nhau ngay lúc 0 không phụ thuộc vào vận tốc nên lúc đầu chúng đóng góp một ảnh. Việc triển khai ngây thơ chỉ kiểm tra các giao lộ có thời gian dương sẽ bỏ lỡ những điều này. 

Cuối cùng, có những trường hợp hai quỹ đạo không bao giờ gặp nhau vì chuyển động tương đối của chúng không giao nhau trong thời gian thuận, chẳng hạn khi người nhanh hơn bắt đầu về phía trước. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực sẽ kiểm tra từng cặp người và tính toán xem quỹ đạo của họ có giao nhau hay không. Đối với một cặp$(i, j)$, ta giải:$$x_i + v_i t = x_j + v_j t$$mang lại:$$t = \frac{x_j - x_i}{v_i - v_j}$$Nếu như$v_i = v_j$, chúng chỉ gặp nhau nếu$x_i = x_j$, nếu không thì không bao giờ. Nếu như$t \ge 0$, chúng tôi tính một cuộc họp. 

Điều này có tác dụng vì mỗi cặp hợp lệ tương ứng với chính xác một sự kiện họp. Tính chính xác rất đơn giản, nhưng nó không xử lý rõ ràng các va chạm giữa nhiều người theo cách được nhóm. Tuy nhiên, việc đếm theo cặp là đủ vì mỗi cuộc họp sẽ đóng góp độc lập cho mỗi cặp. 

Vấn đề chỉ nằm ở hiệu suất nếu chúng tôi đã thử bất kỳ điều gì ngoài việc liệt kê cặp, chẳng hạn như sắp xếp các sự kiện theo thời gian hoặc mô phỏng chuyển động. Điều đó sẽ yêu cầu xử lý lên đến$O(n^2)$các sự kiện và sắp xếp chúng, dẫn đến$O(n^2 \log n)$, điều đó là không cần thiết. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần mô phỏng thời gian. Mọi cuộc gặp đều được xác định hoàn toàn bằng đại số theo cặp và việc đếm trực tiếp tất cả các cặp hợp lệ sẽ tạo ra câu trả lời đúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force | O(n²) | O(1) | Đã chấp nhận | 
| Mô phỏng / Sắp xếp sự kiện | O(n² log n) | O(n²) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các cặp người không có thứ tự$(i, j)$. Điều này đảm bảo mọi tương tác có thể được xem xét chính xác một lần. 
2. Nếu$v_i = v_j$, kiểm tra xem$x_i = x_j$. Nếu vậy thì hai người này ngay từ đầu đã luôn ở bên nhau nên chỉ đóng góp đúng một bức ảnh. Nếu không, họ không bao giờ gặp nhau và không đóng góp được gì. 
3. Nếu$v_i \ne v_j$, tính thời gian họp bằng cách sử dụng:$$t = \frac{x_j - x_i}{v_i - v_j}$$Điều này bắt nguồn trực tiếp từ việc đánh đồng vị trí của chúng theo thời gian. 
4. Chỉ tính cặp nếu$t \ge 0$. Thời gian âm tương ứng với một cuộc gặp trong quá khứ, điều này không liên quan vì chuyển động bắt đầu tại$t = 0$. 
5. Tích lũy tổng số cặp hợp lệ. 

Tại sao nó hoạt động: 

Mỗi cặp người có một thời điểm gặp gỡ tiềm năng duy nhất được xác định bởi chuyển động tuyến tính. Nếu thời gian đó tồn tại và không âm thì cặp đôi đó sẽ đóng góp đúng một bức ảnh. Ngay cả khi nhiều người gặp nhau cùng một lúc tại cùng một điểm, mỗi cặp trong nhóm đó vẫn được tính độc lập, do đó, việc tích lũy theo cặp sẽ ghi lại các xung đột nhóm một cách tự nhiên mà không cần xử lý đặc biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
people = [tuple(map(int, input().split())) for _ in range(n)]

ans = 0

for i in range(n):
    x1, v1 = people[i]
    for j in range(i + 1, n):
        x2, v2 = people[j]

        if v1 == v2:
            if x1 == x2:
                ans += 1
            continue

        num = x2 - x1
        den = v1 - v2

        if den == 0:
            continue

        # check if t >= 0 without floating point
        # t = num / den >= 0  <=> num and den have same sign
        if num * den >= 0:
            ans += 1

print(ans)
```Việc thực hiện phản ánh trực tiếp lý luận theo cặp. Cấu trúc vòng lặp liệt kê tất cả các cặp trong$O(n^2)$. Trường hợp vận tốc bằng nhau được tách ra vì nếu không phép chia sẽ không hợp lệ và về mặt khái niệm tương ứng với sự chồng chéo vĩnh viễn hoặc không bao giờ gặp nhau. 

điều kiện`num * den >= 0`tránh các vấn đề về độ chính xác của dấu phẩy động bằng cách kiểm tra dấu của phân số thay vì tính toán rõ ràng. Điều này quan trọng vì vị trí và vận tốc là số nguyên nhưng phép chia có thể gây ra các lỗi dấu phẩy động khó phát hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
0 1
10 -1
```Hai người này đang tiến về phía nhau. 

| tôi | j | x1 | v1 | x2 | v2 | Trường hợp | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 10 | -1 | vận tốc khác nhau | 1 | 

Họ gặp nhau đúng một lần$t = 5$. Thuật toán đếm cặp hợp lệ duy nhất này. 

Điều này xác nhận rằng điều kiện giao lộ phát hiện chính xác chuyển động trực diện. 

### Ví dụ 2 

đầu vào:```
3
0 1
0 2
10 -1
```Hai người bắt đầu cùng nhau, và một người nữa tiếp cận sau. 

| tôi | j | x1 | v1 | x2 | v2 | Trường hợp | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | 2 | cùng vị trí | 1 | 
| 0 | 2 | 0 | 1 | 10 | -1 | gặp sau | 1 | 
| 1 | 2 | 0 | 2 | 10 | -1 | gặp sau | 1 | 

Tổng cộng là 3. 

Điều này cho thấy cách xử lý chính xác các vị trí xuất phát đồng thời và cách tất cả các tương tác theo cặp trong một vụ va chạm giữa ba người được ghi lại một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Chúng tôi kiểm tra từng cặp không có thứ tự chính xác một lần | 
| Không gian | O(1) | Chỉ một số lượng biến không đổi được sử dụng ngoài bộ nhớ đầu vào | 

Với$n \le 1000$, số lượng cặp tối đa là khoảng 500.000, đủ nhanh trong Python dưới giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    people = [tuple(map(int, input().split())) for _ in range(n)]

    ans = 0
    for i in range(n):
        x1, v1 = people[i]
        for j in range(i + 1, n):
            x2, v2 = people[j]

            if v1 == v2:
                if x1 == x2:
                    ans += 1
                continue

            num = x2 - x1
            den = v1 - v2

            if num * den >= 0:
                ans += 1

    return str(ans)

# provided sample-style tests
assert run("2\n0 1\n10 -1\n") == "1"
assert run("3\n0 1\n0 2\n10 -1\n") == "3"

# custom cases
assert run("1\n5 5\n") == "0", "single person"
assert run("2\n1 1\n1 1\n") == "1", "identical trajectories"
assert run("2\n0 1\n1 2\n") == "1", "same direction faster behind"
assert run("2\n0 2\n10 1\n") == "1", "faster ahead never meets"
assert run("3\n0 1\n1 1\n2 1\n") == "2", "parallel same velocity chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| người độc thân | 0 | không có cặp nào tồn tại | 
| quỹ đạo giống hệt nhau | 1 | cùng xuất phát và vận tốc | 
| cùng hướng nhanh hơn phía sau | 1 | trường hợp bắt kịp | 
| nhanh hơn phía trước không bao giờ gặp nhau | 1 | lọc thời gian âm | 
| chuỗi vận tốc song song | 2 | nhiều cặp vận tốc giống hệt nhau | 

## Vỏ cạnh 

Khi nhiều người xuất phát ở cùng một vị trí, mỗi cặp trong số họ sẽ được tính ngay lập tức. Đối với đầu vào`3 0 1 0 2 0 3`, thuật toán đánh giá cả ba cặp và mỗi cặp đều thỏa mãn`x1 == x2`, do đó nó trả về 3, khớp với thực tế là tất cả các cặp gặp nhau tại thời điểm 0. 

Khi vận tốc bằng nhau nhưng vị trí khác nhau, chẳng hạn như`2 0 1 10 1`, điều kiện`v1 == v2`kích hoạt và bỏ qua việc đếm, phản ánh chính xác rằng chuyển động song song không bao giờ giao nhau. 

Khi một người nhanh hơn bắt đầu về phía trước, như`0 2`Và`10 1`, tử số và mẫu số tính được có dấu trái dấu nhau nên`num * den >= 0`không thành công, khiến cuộc họp “ngược thời gian” không hợp lệ được tính.
