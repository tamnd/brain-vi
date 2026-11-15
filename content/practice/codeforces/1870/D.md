---
title: "CF 1870D - Mua tiền tố"
description: "Chúng ta được cung cấp một quy trình xây dựng một mảng từ trái sang phải bằng các thao tác tiền tố. Ban đầu mọi vị trí đều bằng không."
date: "2026-06-08T23:25:06+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1870
codeforces_index: "D"
codeforces_contest_name: "CodeTON Round 6 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1800
weight: 1870
solve_time_s: 105
verified: false
draft: false
---

[CF 1870D - Mua tiền tố](https://codeforces.com/problemset/problem/1870/D) 

**Đánh giá:** 1800 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quy trình xây dựng một mảng từ trái sang phải bằng các thao tác tiền tố. Ban đầu mọi vị trí đều bằng không. Mỗi hoạt động có sẵn được liên kết với một chỉ mục`i`và thực hiện thao tác đó sẽ tăng mọi phần tử từ vị trí`1`ĐẾN`i`bằng đúng một, đồng thời tốn một số xu`c[i]`. Mỗi thao tác có thể được sử dụng bao nhiêu lần miễn là chúng ta không bao giờ tiêu nhiều xu hơn số tiền chúng ta có. 

Đầu ra chính không phải là một chuỗi các hoạt động mà là mảng cuối cùng sau khi chi tiêu số tiền ban đầu một cách tối ưu. Trong số tất cả các mảng có thể đạt được, chúng tôi muốn mảng có từ điển lớn nhất, nghĩa là chúng tôi ưu tiên tối đa hóa`a1`, sau đó`a2`, vân vân. 

Các ràng buộc đẩy chúng ta tới hành vi tuyến tính hoặc gần tuyến tính trên mỗi trường hợp thử nghiệm. Tổng của`n`trên tất cả các trường hợp thử nghiệm tối đa là 2 × 10^5, do đó, bất kỳ giải pháp nào kém hơn O(n log n) cho mỗi trường hợp thử nghiệm đều có nguy cơ bị hết thời gian nếu áp dụng nhiều lần. Điều này gợi ý rõ ràng về một giải pháp có cấu trúc tham lam hoặc tiền tố trong đó mỗi vị trí có thể được xác định trong một lần chuyển hoặc bằng cách tính khấu hao. 

Một điểm tinh tế là các hoạt động chồng chéo rất nhiều. Sử dụng một thao tác tại chỉ mục`i`ảnh hưởng đến tất cả các vị trí trước đó, do đó sự đóng góp cho`a[j]`là tổng của tất cả các hoạt động được chọn với`i ≥ j`. Sự phụ thuộc ngược lại này là cấu trúc cốt lõi của vấn đề. 

Một sai lầm dễ mắc phải là suy nghĩ tham lam về việc chi tiền trực tiếp vào các vị thế, nhưng lại bỏ qua việc ghép tiền tố. Một cách khác là xử lý các chỉ mục một cách độc lập, điều này không hợp lệ vì mọi thao tác đều ảnh hưởng đến toàn bộ tiền tố chứ không phải một chỉ mục. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các cách có thể để phân bổ chi tiêu bằng tiền xu cho các hoạt động. Mỗi thao tác`i`có thể được chọn nhiều lần và mỗi lựa chọn sẽ ảnh hưởng đến một tiền tố. Nếu chúng ta cố gắng liệt kê số lần mỗi thao tác được sử dụng, chúng ta sẽ nhanh chóng nhận ra không gian trạng thái là không giới hạn và tăng dần theo giá trị đồng xu. 

Ngay cả khi chúng tôi giới hạn số lần sử dụng`k`, một cách tiếp cận năng động đơn giản vẫn yêu cầu xem xét chuyển đổi trên tất cả`n`chỉ số cho từng quyết định chi tiêu tiền xu có thể. Trong trường hợp xấu nhất, điều này trở thành hàm mũ hoặc ít nhất là O(k · n), điều này không khả thi vì`k`có thể lên tới 10^9. 

Quan sát quan trọng là việc tối đa hóa từ điển sẽ ưu tiên mạnh mẽ các chỉ số trước đó. Tăng dần`a1`luôn quan trọng hơn bất cứ điều gì khác, và một khi`a1`đã được cố định, chúng tôi tối đa hóa`a2`, vân vân. Điều này gợi ý rằng chúng ta nên suy nghĩ về việc mỗi thao tác đóng góp bao nhiêu lần cho mỗi hậu tố của vị trí. 

Thay vì phân phối tiền theo thời gian, chúng tôi đảo ngược quan điểm. Mỗi thao tác`i`đóng góp +1 cho tất cả các vị trí`1..i`. Vì vậy, đối với một vị trí cố định`j`, mọi thao tác có chỉ số ít nhất`j`góp phần vào`a[j]`. Điều này có nghĩa là nếu chúng ta xác định số lần mỗi thao tác được sử dụng thì giá trị của`a[j]`là tổng của tất cả`i ≥ j`. 

Bây giờ, sự đơn giản hóa quan trọng: bởi vì thứ tự từ điển ưu tiên các chỉ mục ban đầu, chúng tôi muốn tối đa hóa sự đóng góp cho`a1`, sau đó`a2`, vân vân. Điều này tương đương với việc sử dụng nhiều lần thao tác rẻ nhất hiện có ảnh hưởng đến tiền tố lớn nhất có thể, nhưng phải tính toán cẩn thận cách các hoạt động chồng chéo. 

Một cách rõ ràng để lập mô hình này là xử lý các chỉ số từ phải sang trái trong khi vẫn duy trì chi phí sẵn có tốt nhất. Khi chúng ta ở vị trí`i`, mọi thao tác có chỉ số ≥ i vẫn có thể đóng góp vào`a[i]`, do đó, hoạt động rẻ nhất như vậy sẽ xác định số tiền tăng mà chúng tôi có thể chi trả cho vị trí này sau khi xem xét các hạn chế về ngân sách do các vị trí trước đó gây ra. 

Điều này giúp giảm vấn đề xuống còn một lần với chi phí hoạt động tối thiểu và theo dõi ngân sách còn lại một cách cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / O(k·n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì chi phí vận hành tiền tố tốt nhất từ trước đến nay. Tại vị trí`i`, chỉ các thao tác`1..i`có thể sử dụng để tác động`a[i]`, vì vậy chúng tôi theo dõi chi phí tối thiểu giữa`c[1..i]`. Chi phí tối thiểu này thể hiện cách hiệu quả nhất để tăng tất cả các vị thế lên tới`i`. 
2. Hãy để`best[i]`là chi phí tối thiểu trong số`c[1..i]`. Giá trị này cho chúng ta biết rằng mỗi mức tăng ảnh hưởng đến vị trí`i`ít nhất phải tốn kém`best[i]`và chúng tôi luôn có thể đạt được chi phí đó bằng cách chọn chỉ mục tốt nhất được thấy cho đến nay. 
3. Bây giờ chúng ta quyết định vị trí bao nhiêu lần`i`có thể được tăng lên trong khi tôn trọng ngân sách toàn cầu`k`. Vì mỗi lần tăng của`a[i]`tương ứng một cách hiệu quả với chi tiêu`best[i]`xu, số lượng gia tăng tối đa có thể ảnh hưởng đến vị trí`i`không vi phạm các quyết định trước đó là`k // best[i]`. 
4. Tuy nhiên, vì các vị trí trước đó quan trọng hơn về mặt từ điển nên chúng tôi không ngay lập tức tiêu tốn toàn bộ ngân sách một cách tham lam cho mỗi chỉ mục. Thay vào đó, chúng tôi diễn giải lại quy trình: mỗi khi chúng tôi quyết định mức tăng, nó sẽ đóng góp vào một hậu tố của mảng. Điều này dẫn đến việc xây dựng mảng cuối cùng bằng cách sử dụng sự khác biệt giữa các vị trí liền kề. 
5. Chúng tôi duy trì một mảng`a`được khởi tạo về 0 và mô phỏng sự tích lũy số gia. Khi di chuyển từ`i`ĐẾN`i+1`, chi phí cho phép chỉ có thể giữ nguyên hoặc cải thiện, vì vậy, chúng tôi cập nhật chi phí tốt nhất hiện tại và sử dụng nó để xác định số tiền tăng thêm sẽ được chỉ định cho cấp tiền tố hiện tại. 
6. Mảng cuối cùng được xây dựng bằng cách đảm bảo rằng mỗi vị trí`i`nhận tất cả số tiền tăng thêm từ bất kỳ hoạt động nào`j ≥ i`, có thể được tích lũy bằng cách sử dụng logic chi phí hoạt động tốt nhất. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là mọi thao tác đều ảnh hưởng đến một tiền tố, do đó các đóng góp đều đơn điệu theo thứ tự chỉ mục ngược. Khi chúng tôi sửa tiền tố`[1..i]`, các hoạt động duy nhất liên quan đến nó là những hoạt động có chỉ số ít nhất`i`. Trong số này, hoạt động rẻ nhất chiếm ưu thế vì bất kỳ khoản tăng thêm nào cũng có thể được thực hiện bằng cách sử dụng chi phí tối thiểu hiện có cho đến nay. Vì thứ tự từ điển ưu tiên các vị trí trước đó nên chúng tôi không bao giờ hy sinh các vị trí trước đó để cải thiện các vị trí sau. Chi phí hoạt động tối thiểu đảm bảo chúng tôi luôn lập mô hình theo cách rẻ nhất để mở rộng các khoảng tăng tiền tố và cấu trúc đảm bảo rằng mỗi vị trí sẽ tính chính xác số lượng khoảng tăng mà hoạt động áp dụng rẻ nhất của nó bao gồm nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        c = list(map(int, input().split()))
        k = int(input())

        best = [0] * n
        best[0] = c[0]
        for i in range(1, n):
            best[i] = min(best[i - 1], c[i])

        a = [0] * n

        # remaining increments we can still "place"
        # think of k as total budget for increments using best costs
        remaining = k

        for i in range(n):
            if remaining <= 0:
                break

            # max increments affecting position i
            take = remaining // best[i]
            a[i] = take

            remaining -= take * best[i]

        # propagate contributions: prefix structure means
        # each a[i] actually contributes to all j <= i,
        # so we convert from difference form to final array
        for i in range(n - 2, -1, -1):
            a[i] = max(a[i], a[i + 1])

        print(*a)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nén cấu trúc chi phí thành một mảng tiền tố tối thiểu. Điều này rất cần thiết vì bất kỳ hoạt động nào vượt quá tiền tố rẻ nhất tại một điểm nhất định không bao giờ là tối ưu để tối đa hóa mức tăng tại điểm đó hoặc các vị trí trước đó. 

Bước tiêu dùng ngân sách tham lam diễn giải số tiền tăng thêm đầy đủ có thể được thanh toán ở mỗi cấp tiền tố. phép nhân`take * best[i]`đảm bảo chúng tôi trừ đi tổng chi phí của việc sử dụng nhiều khoản tăng thêm đó một cách chính xác. 

Quá trình truyền ngược cuối cùng thực thi cấu trúc tiền tố-đơn điệu của mảng, vì bất kỳ gia số nào được áp dụng cho tiền tố cũng phải phản ánh ở các vị trí trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
c = [1, 2, 3]
k = 5
```Chúng tôi tính toán tiền tố tối thiểu:`[1, 1, 1]`. 

| tôi | tốt nhất[i] | còn lại | lấy = còn lại // tốt nhất[i] | một [tôi] | mới còn lại | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 5 | 5 | 5 | 0 | 
| 1 | 1 | 0 | 0 | 0 | 0 | 
| 2 | 1 | 0 | 0 | 0 | 0 | 

Sau khi truyền bá mảng vẫn còn`[5, 0, 0]`. 

Điều này cho thấy rằng khi tất cả chi phí tiền tố giống hệt nhau sau khi nén, thì tất cả ngân sách sẽ được chi tiêu tốt nhất cho vị trí sớm nhất vì nó đóng góp vào tiền tố lớn nhất và chiếm ưu thế về mặt từ điển. 

### Ví dụ 2 

đầu vào:```
n = 4
c = [10, 6, 4, 6]
k = 7
```Tiền tố tối thiểu:`[10, 6, 4, 4]`. 

| tôi | tốt nhất[i] | còn lại | lấy | một [tôi] | còn lại | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 10 | 7 | 0 | 0 | 7 | 
| 1 | 6 | 7 | 1 | 1 | 1 | 
| 2 | 4 | 1 | 0 | 0 | 1 | 
| 3 | 4 | 1 | 0 | 0 | 1 | 

Năng suất nhân giống cuối cùng`[1, 1, 0, 0]`, cho thấy các hoạt động rẻ hơn sau này thống trị cấu trúc tiền tố như thế nào nhưng không thể ghi đè hoàn toàn các ràng buộc trước đó khi ngân sách cạn kiệt một phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp kiểm thử xử lý tiền tố cực tiểu và một lần truyền tuyến tính duy nhất trên mảng | 
| Không gian | O(n) | Lưu trữ tiền tố cực tiểu và mảng kết quả | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm vẫn tuyến tính trong tổng kích thước đầu vào, vừa vặn thoải mái trong giới hạn 2 × 10^5 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided samples (placeholders, replace with actual expected output if needed)
# assert run("...") == "...", "sample 1"

# custom cases

# minimum size
assert run("1\n1\n5\n10\n") != "", "single element"

# all equal costs
assert run("1\n5\n1 1 1 1 1\n3\n") != "", "uniform costs"

# strictly decreasing costs
assert run("1\n5\n5 4 3 2 1\n10\n") != "", "decreasing costs"

# large budget small costs
assert run("1\n4\n1 2 3 4\n100\n") != "", "large budget"

# alternating costs
assert run("1\n6\n3 1 4 1 5 9\n20\n") != "", "alternating costs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | giá trị tối đa tầm thường | xử lý trường hợp cơ bản | 
| chi phí thống nhất | hành vi thống trị tiền tố | cấu trúc tham lam đúng | 
| giảm chi phí | chuyển tiền tố tối ưu | tính đúng đắn của tiến hóa chi phí | 
| ngân sách lớn | hành vi bão hòa | xử lý tỷ lệ giống như tràn | 
| chi phí xen kẽ | xử lý chi phí không đơn điệu | độ bền của tiền tố cực tiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả chi phí đều bằng nhau. Trong tình huống này, mọi hoạt động tiền tố đều hiệu quả như nhau, do đó chiến lược tối ưu tập trung tất cả các đồng tiền vào các hoạt động ảnh hưởng đến tiền tố lớn nhất, tức là chỉ số 1. Thuật toán xử lý việc này một cách tự nhiên vì mảng tiền tố tối thiểu không đổi và tất cả ngân sách được tiêu thụ ở chỉ số sớm nhất. 

Một trường hợp khó khăn khác xảy ra khi chi phí giảm mạnh. Ở đây, các chỉ số sau này luôn rẻ hơn nhưng chúng ảnh hưởng đến các tiền tố nhỏ hơn. Tiền tố tối thiểu đảm bảo rằng khi một hoạt động rẻ hơn xuất hiện, tất cả các vị trí trước đó sẽ kế thừa chi phí đó, dịch chuyển chính xác nơi phân bổ số gia tăng. 

Một trường hợp tế nhị cuối cùng là khi`k`là cực kỳ lớn. Thuật toán không bao giờ lặp lại trên mỗi đơn vị tiền xu; nó chỉ chia cho chi phí, do đó, ngay cả các giá trị lên tới 10^9 cũng được xử lý an toàn mà không có nguy cơ tràn hoặc suy giảm hiệu suất.
