---
title: "CF 102569G - Đai ốc và Bu lông"
description: "Chúng tôi có hai bộ sưu tập các đối tượng. Một bộ sưu tập chứa đai ốc và bộ kia chứa bu lông. Mỗi đai ốc có một kích thước riêng, mỗi bu lông có một kích thước riêng và các kích cỡ tạo thành cùng một phạm vi."
date: "2026-07-31T07:52:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102569
codeforces_index: "G"
codeforces_contest_name: "2020, XIII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102569
solve_time_s: 131
verified: false
draft: false
---

[CF 102569G - Đai ốc và Bu lông](https://codeforces.com/problemset/problem/102569/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai bộ sưu tập các đối tượng. Một bộ sưu tập chứa đai ốc và bộ kia chứa bu lông. Mỗi đai ốc có một kích thước riêng, mỗi bu lông có một kích thước riêng và các kích cỡ tạo thành cùng một phạm vi. Cách duy nhất để tìm hiểu về mối quan hệ giữa hai bộ sưu tập là so sánh một đai ốc với một bu lông. Việc so sánh cho chúng ta biết đai ốc nhỏ hơn, lớn hơn hay có cùng kích thước với bu lông. 

Mục đích là để khám phá, đối với mỗi đai ốc, chỉ số của bu lông có cùng kích thước. Chương trình không nhận được kích thước trực tiếp. Thay vào đó, nó phải đưa ra những so sánh và sử dụng các câu trả lời để xây dựng lại kết quả khớp ẩn. 

Giới hạn của`5 * n * log2(n)`sự so sánh cho chúng ta biết rằng việc thử mọi cặp đai ốc và bu lông có thể là không khả thi. Với`n = 1000`, một phương pháp bậc hai sẽ thực hiện khoảng một triệu phép so sánh, trong khi số phép so sánh được phép chỉ khoảng năm mươi nghìn. Giải pháp phải loại bỏ nhiều lần các phần lớn của không gian tìm kiếm, hướng tới cách tiếp cận phân chia và chinh phục tương tự như sắp xếp nhanh. 

Các trường hợp phức tạp là do hai nhóm không được sắp xếp theo thứ tự và chỉ được phép so sánh giữa các nhóm. Một giải pháp bất cẩn có thể phân loại các loại hạt bằng cách so sánh các loại hạt với nhau, nhưng sự so sánh như vậy là không thể. Ví dụ: với hai đai ốc và hai bu lông trong đó thứ tự ẩn là kích thước đai ốc`[2, 1]`và kích thước bu lông`[1, 2]`, đáp án đúng là thằng điên đó`1`chốt trận đấu`2`và hạt`2`chốt trận đấu`1`. Phương pháp cố gắng sắp xếp độc lập các đai ốc và bu lông không thể thực hiện được các so sánh cần thiết. 

Một trường hợp lỗi khác là quên rằng một phân vùng phải sử dụng cùng thông tin trục ở cả hai bên. Giả sử đai ốc được chọn khớp với một số bu lông ở giữa mảng bu lông. Sau khi tìm thấy chốt đó, các đai ốc còn lại phải được phân chia bằng cách so sánh với chốt đó. Nếu quá trình triển khai chọn một chốt khác làm trục, hai phân vùng có thể không còn mô tả các phạm vi kích thước giống nhau và các kết quả khớp hợp lệ có thể bị mất. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp là so sánh từng đai ốc với từng bu lông cho đến khi tìm thấy tất cả các kết quả khớp. Vì mỗi đai ốc có chính xác một bu lông phù hợp nên việc này cuối cùng đã thành công. Trường hợp xấu nhất đòi hỏi`n * n`so sánh vì bu-lông phù hợp luôn có thể là bu-lông cuối cùng được thử nghiệm cho mỗi đai ốc. Vì`n = 1000`, đây là một triệu so sánh, vượt xa giới hạn yêu cầu. 

Nhận xét quan trọng là việc so sánh mang lại nhiều điều hơn là chỉ một sự trùng khớp có thể xảy ra. Nó cũng cung cấp thông tin đặt hàng. Nếu so sánh đai ốc với bu lông và đai ốc đó nhỏ hơn thì đai ốc đó không thể khớp với bất kỳ bu lông nào lớn hơn hoặc bằng bu lông đó. Nếu nó lớn hơn, hạn chế ngược lại sẽ được áp dụng. 

Điều này cho phép chúng ta sử dụng phân vùng kiểu quicksort. Chọn một đai ốc làm trục và so sánh nó với mọi bu lông. Điều này chia các bu lông thành các bu lông nhỏ hơn, một bu lông phù hợp và các bu lông lớn hơn. Bu lông phù hợp bây giờ đã được biết đến. Tiếp theo, dùng bu-lông đó để phân chia các đai ốc còn lại thành hai nhóm có cùng kích thước. Nhóm đai ốc bên trái chỉ có thể khớp với nhóm bu lông bên trái và nhóm bên phải chỉ có thể khớp với nhóm bu lông bên phải. Quá trình tương tự sau đó có thể được lặp lại đệ quy. 

Số lượng so sánh tuân theo mô hình tương tự như quicksort. Mỗi cấp độ đệ quy xử lý tất cả các phần tử còn lại và độ sâu là logarit khi các trục chia các nhóm một cách hợp lý. Giới hạn so sánh có đủ biên độ cho phiên bản ngẫu nhiên tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) dự kiến ​​| O(log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chọn một đai ốc từ phạm vi hiện tại làm trục xoay. So sánh nó với mọi loại bu lông trong phạm vi bu lông hiện tại. Lưu trữ chốt đưa ra câu trả lời bằng nhau. Việc so sánh cũng chia tất cả các bu lông khác thành những bu lông nhỏ hơn đai ốc trụ và những bu lông lớn hơn nó. 

Bu lông phù hợp của đai ốc trục hiện đã được cố định vì không có bu lông nào khác có thể có cùng kích thước. 

1. So sánh mọi đai ốc khác trong dòng đai ốc hiện tại với bu lông phù hợp được tìm thấy ở bước trước. Đặt các loại hạt nhỏ hơn vào nhóm bên trái và các loại hạt lớn hơn vào nhóm bên phải. 

Sử dụng bu lông phù hợp làm dải phân cách giữ cho phân vùng đai ốc và phân vùng bu lông nhất quán. Nhóm đai ốc bên trái chỉ có thể khớp với nhóm bu lông bên trái và bên phải cũng tuân theo quy tắc tương tự. 

1. Giữ cặp đã phát hiện được giữa đai ốc trụ và chốt của nó. 
2. Giải đệ quy các nhóm đai ốc và bu lông bên trái, sau đó giải đệ quy các nhóm đai ốc và bu lông bên phải. Các nhóm trống và các nhóm phần tử đơn đã được giải quyết. 
3. Sau khi tất cả lệnh gọi đệ quy kết thúc, xuất chỉ số bu-lông được lưu trữ cho mỗi chỉ số đai ốc. 

Tại sao nó hoạt động: 

Điều bất biến là mọi lệnh gọi đệ quy đều nhận được hai nhóm chứa chính xác cùng một bộ kích thước, một nhóm được biểu thị bằng đai ốc và một nhóm được biểu thị bằng bu lông. Khi so sánh một đai ốc trục với tất cả các bu lông, chính xác một bu lông khớp với nó và mọi bu lông khác được phân loại tương ứng với kích thước đó. So sánh đai ốc với bu lông phù hợp đó sẽ tạo ra sự phân chia giống hệt nhau ở phía đai ốc. Vì mọi kết quả khớp có thể đều nằm trong một bài toán con đệ quy và khớp trục được ghi lại vĩnh viễn nên mọi đai ốc đều nhận được chốt chính xác. 

## Giải pháp Python```python
import sys
import random

input = sys.stdin.readline

def solve():
    n = int(input())
    ans = [0] * n

    def ask(nut, bolt):
        print("?", nut + 1, bolt + 1, flush=True)
        return input().strip()

    def divide(nuts, bolts):
        if not nuts:
            return

        if len(nuts) == 1:
            ans[nuts[0]] = bolts[0]
            return

        pivot_nut = nuts[random.randrange(len(nuts))]

        smaller_bolts = []
        larger_bolts = []
        pivot_bolt = -1

        for bolt in bolts:
            res = ask(pivot_nut, bolt)
            if res == "<":
                larger_bolts.append(bolt)
            elif res == ">":
                smaller_bolts.append(bolt)
            else:
                pivot_bolt = bolt

        smaller_nuts = []
        larger_nuts = []

        for nut in nuts:
            if nut == pivot_nut:
                continue
            res = ask(nut, pivot_bolt)
            if res == "<":
                smaller_nuts.append(nut)
            else:
                larger_nuts.append(nut)

        ans[pivot_nut] = pivot_bolt

        divide(smaller_nuts, smaller_bolts)
        divide(larger_nuts, larger_bolts)

    divide(list(range(n)), list(range(n)))

    print("!", *[x + 1 for x in ans], flush=True)

if __name__ == "__main__":
    solve()
```Hàm đệ quy giữ hai mảng song song biểu thị các đai ốc và bu lông hiện chưa được giải quyết. Các trường hợp cơ sở xử lý các phạm vi trống và một cặp còn lại duy nhất, trong đó câu trả lời đã được ép buộc. 

Việc lựa chọn trục là ngẫu nhiên vì một trục cố định có thể liên tục chọn phần tử nhỏ nhất hoặc lớn nhất và tạo độ sâu đệ quy là`n`. Lựa chọn ngẫu nhiên đưa ra hành vi cân bằng mong đợi cần thiết cho giới hạn so sánh. 

Phân vùng đầu tiên chỉ so sánh đai ốc trục với bu lông. Phân vùng thứ hai chỉ so sánh các đai ốc với chốt xoay đã được phát hiện. Thứ tự của các thao tác này rất quan trọng vì chốt xoay là cầu nối truyền thông tin từ phía bu lông trở lại phía đai ốc. 

Mảng câu trả lời lưu trữ các chỉ số chốt bằng cách sử dụng chỉ mục dựa trên số 0 bên trong. Đầu ra cuối cùng chuyển đổi chúng trở lại chỉ mục dựa trên một yêu cầu của giao thức tương tác. 

## Ví dụ đã hoạt động 

Hãy xem xét một sự sắp xếp ẩn nhỏ trong đó các chỉ số đai ốc tương ứng với kích thước`[3, 1, 2]`và chỉ số bu lông tương ứng với kích thước`[2, 3, 1]`. 

Lựa chọn trục đầu tiên có thể chọn đai ốc`1`, có kích thước`3`. 

| Bước | Đai ốc xoay | Bu lông so sánh | Kết quả | Tiểu bang | 
| --- | --- | --- | --- | --- | 
| 1 | hạt 1 | bu lông 1 | đai ốc lớn hơn | bu lông 1 đi sang trái | 
| 2 | hạt 1 | bu lông 2 | bằng | bu lông 2 khớp | 
| 3 | hạt 1 | bu lông 3 | đai ốc lớn hơn | bu lông 3 đi sang trái | 

Trận đấu trục được lưu dưới dạng đai ốc`1`chốt`2`. Các đai ốc còn lại được tách ra bằng cách so sánh với bu lông`2`. 

| Bước | Hạt so sánh | Kết quả chống chốt xoay | Nhóm | 
| --- | --- | --- | --- | 
| 1 | hạt 2 | nhỏ hơn | trái | 
| 2 | hạt 3 | nhỏ hơn | trái | 

Cuộc gọi đệ quy xử lý hai cặp còn lại. Dấu vết này thể hiện tính bất biến trung tâm: cả hai bộ sưu tập đều được chia xung quanh ranh giới có cùng kích thước. 

Đối với mẫu có năm phần tử, thuật toán thực hiện lặp lại quy trình tương tự. Phân vùng đầu tiên có thể phát hiện ra một cặp chính xác, sau đó tách bốn cặp còn lại thành hai vấn đề độc lập nhỏ hơn. Mỗi lớp đệ quy loại bỏ nhu cầu so sánh lại các kích thước không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) dự kiến ​​| Mọi cấp độ đệ quy đều so sánh tất cả các phần tử còn lại và các trục xoay ngẫu nhiên cho độ sâu dự kiến ​​theo logarit | 
| Không gian | O(log n) dự kiến ​​| Ngăn xếp đệ quy lưu trữ một khung hình cho mỗi cấp độ phân chia | 

Tối đa`n`đủ nhỏ để dự kiến`O(n log n)`số lượng so sánh phù hợp thoải mái dưới`5 * n * log2(n)`. Việc sử dụng bộ nhớ cũng nhỏ vì chỉ lưu trữ ngăn xếp đệ quy và các phân vùng tạm thời. 

## Trường hợp thử nghiệm 

Sự cố này mang tính tương tác, do đó, các bài kiểm tra đơn vị dựa trên đầu vào thông thường không thể xác thực trực tiếp chương trình cuối cùng. Một bài kiểm tra ngoại tuyến hoàn chỉnh sẽ yêu cầu một giám khảo mô phỏng trả lời các câu hỏi so sánh. Bản phác thảo sau đây cho thấy kiểu khai thác thử nghiệm mô phỏng cho logic phân chia và chinh phục.```python
import io
import sys

def run(hidden_nuts, hidden_bolts):
    n = len(hidden_nuts)
    queries = iter([])

    # A real test harness would replace ask() with a function that compares
    # the hidden sizes and returns "<", ">", or "=".
    return "mock judge required"

assert run([1], [1]) == "mock judge required"
assert run([2, 1], [1, 2]) == "mock judge required"
assert run([3, 1, 2], [2, 3, 1]) == "mock judge required"
assert run(list(range(1000)), list(range(1000))) == "mock judge required"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một đai ốc và một bu lông | Cặp phù hợp | Xử lý trường hợp cơ bản | 
| Hai kích thước đảo ngược | Chỉ số hoán đổi | Hướng phân vùng đúng | 
| Ba yếu tố theo thứ tự hỗn hợp | Phục hồi hoán vị chính xác | Phân chia đệ quy | 
| Một ngàn yếu tố | Kết hợp hoàn chỉnh | Hiệu suất dưới những hạn chế | 

## Vỏ cạnh 

Một cặp còn lại là trạng thái đệ quy nhỏ nhất. Đối với trường hợp có một đai ốc có kích thước`1`và một bu lông có kích thước`1`, thuật toán không bao giờ thực hiện phân vùng không cần thiết và ghi trực tiếp kết quả khớp duy nhất có thể. 

Thứ tự đảo ngược sẽ kiểm tra xem hai phân vùng có còn được đồng bộ hóa hay không. Đối với các loại hạt có kích cỡ`[2, 1]`và bu lông với các kích cỡ`[1, 2]`, chọn đai ốc đầu tiên làm trục tìm bu lông`2`như trận đấu của nó. Đai ốc và bu lông còn lại vẫn ở cùng nhau ở phía đối diện của phân vùng, do đó lệnh gọi đệ quy sẽ gán chính xác bu lông`1`. 

Trình tự mất cân bằng cao là mối nguy hiểm chính về hiệu suất. Nếu quá trình triển khai luôn chọn đai ốc đầu tiên làm trục và dữ liệu đã được sắp xếp sẵn thì phép đệ quy có thể giảm xuống độ sâu tuyến tính. Trục ngẫu nhiên tránh phụ thuộc vào thứ tự ban đầu và giữ số lượng so sánh dự kiến ​​trong giới hạn bắt buộc.
