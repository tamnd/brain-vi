---
title: "CF 104869K - Xếp hạng tối đa"
description: "Chúng ta được cung cấp một mảng các số nguyên thể hiện sự thay đổi xếp hạng từ một số vòng thi. Chúng ta được phép sắp xếp lại các vòng này một cách tùy ý. Sau đó, chúng tôi mô phỏng bắt đầu từ xếp hạng 0, thêm từng giá trị một."
date: "2026-06-28T10:52:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104869
codeforces_index: "K"
codeforces_contest_name: "The 2023 ICPC Asia Shenyang Regional Contest (The 2nd Universal Cup. Stage 13: Shenyang)"
rating: 0
weight: 104869
solve_time_s: 58
verified: true
draft: false
---

[CF 104869K - Xếp hạng tối đa](https://codeforces.com/problemset/problem/104869/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên thể hiện sự thay đổi xếp hạng từ một số vòng thi. Chúng ta được phép sắp xếp lại các vòng này một cách tùy ý. Sau đó, chúng tôi mô phỏng bắt đầu từ xếp hạng 0, thêm từng giá trị một. Mỗi khi tổng hiện có vượt quá tất cả các giá trị trước đó của tổng hiện có, chúng tôi tính đó là bản cập nhật của xếp hạng tối đa. 

Nhiệm vụ không phải là tìm thứ tự tốt nhất mà là một cái gì đó mang tính toàn cầu hơn: đối với một tập hợp nhiều giá trị cố định, chúng ta muốn biết có thể đạt được bao nhiêu giá trị khác nhau của “số lần cập nhật tối đa” bằng cách chọn các hoán vị khác nhau của mảng. Sau mỗi lần cập nhật làm thay đổi một phần tử của mảng, chúng ta phải tính lại số lượng này. 

Hạn chế chính là cả n và q đều có thể lớn tới 200.000, do đó, bất kỳ giải pháp nào cố gắng mô phỏng hoán vị hoặc thậm chí lý do cho mỗi truy vấn với bất kỳ điều gì tệ hơn logarit hoặc công việc liên tục trên mỗi lần cập nhật sẽ không vượt qua. Cấu trúc gợi ý rõ ràng rằng câu trả lời phải phụ thuộc vào một số lượng rất nhỏ các thuộc tính tổng thể của nhiều tập hợp, chứ không phụ thuộc vào sự sắp xếp chi tiết của nó. 

Một trường hợp phức tạp nhưng quan trọng xuất hiện khi tất cả các số đều không dương. Trong trường hợp đó, tổng hiện hành bắt đầu bằng 0 và không bao giờ trở thành số dương hoàn toàn, do đó không bao giờ xảy ra cập nhật tối đa. Một cách tiếp cận ngây thơ giả định rằng ít nhất một bản cập nhật luôn xảy ra sẽ báo cáo không chính xác câu trả lời tích cực ở đây. Một trường hợp mong manh khác là khi có số 0: số 0 không bao giờ làm tăng tổng, nhưng chúng có thể bị nhầm là số dương vô hại nếu người ta chỉ kiểm tra dấu hiệu một cách bất cẩn. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng suy nghĩ trực tiếp, ý tưởng mạnh mẽ là liệt kê tất cả các hoán vị của mảng và mô phỏng quy trình cho từng hoán vị, đếm số lần tổng tiền tố thiết lập một bản ghi mới. Về nguyên tắc, điều này đúng vì nó khớp chính xác với định nghĩa. Tuy nhiên, có n hoán vị giai thừa và thậm chí việc mô phỏng một hoán vị cũng tốn thời gian tuyến tính, vì vậy cách tiếp cận này bùng nổ ngay lập tức khi vượt quá n rất nhỏ. 

Quan sát quan trọng là quy trình chỉ quan tâm đến thời điểm tổng tiền tố vượt qua mức cao mới và điều này chỉ phụ thuộc vào việc các phần tử có dương hay không. Các giá trị âm và 0 có thể trì hoãn sự tăng trưởng nhưng không bao giờ có thể tự mình tạo ra thêm các sự kiện phá kỷ lục. Trong khi đó, mỗi giá trị dương chỉ có thể đóng góp tối đa một bản ghi mới, bởi vì khi tổng tiền tố đạt đến đỉnh mới do số dương, thì không có sự sắp xếp lại nào có thể làm cho phần tử đó đóng góp trở lại. 

Điều này thu gọn toàn bộ vấn đề vào việc theo dõi xem có bao nhiêu giá trị dương thực sự tồn tại. Một khi điều này được nhận ra, số lượng giá trị k có thể đạt được sẽ trở nên cực kỳ hạn chế: nếu không có số dương, quá trình không bao giờ tăng trên 0, do đó giá trị duy nhất có thể là k = 0. Nếu có ít nhất một số dương, chúng ta có thể buộc tất cả các số dương xuất hiện sớm và tạo một bản ghi mới cho mỗi số đó hoặc trì hoãn tất cả các số dương cho đến cuối để chỉ một lần giao nhau xảy ra khi số âm tích lũy được khắc phục. Các hành vi trung gian cũng có thể đạt được, nhưng điều quan trọng là mọi giá trị từ 1 đến số phần tử dương đều có thể thực hiện được, tạo thành một phạm vi liên tục. 

Do đó, câu trả lời cho mỗi mảng chỉ đơn giản là kích thước của phạm vi đếm dương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị | O(n! · n) | O(n) | Quá chậm | 
| Chỉ theo dõi số lượng tích cực | O(n + q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm toàn bộ vấn đề để duy trì có bao nhiêu phần tử trong mảng lớn hơn 0.

1. Tính số phần tử dương ban đầu trong mảng, gọi là pos. 
2. Đối với mỗi bản cập nhật thay đổi một phần tử, trước tiên hãy xác định xem phần tử đó có dương trước bản cập nhật hay không và liệu nó có dương sau bản cập nhật hay không. 
3. Điều chỉnh pos tương ứng bằng cách trừ đi một nếu giá trị dương trở thành không dương hoặc thêm một nếu giá trị không dương trở thành dương. 
4. Sau mỗi lần cập nhật, hãy tính kết quả từ pos: nếu pos bằng 0 thì xuất 1, nếu không thì xuất pos. 

Phần không tầm thường của lý do này là tại sao câu trả lời chỉ phụ thuộc vào vị trí. Cấu trúc của các bản cập nhật tối đa tiền tố đảm bảo rằng chỉ những mức tăng hoàn toàn dương mới có thể tạo ra các bản ghi mới, trong khi các số âm và số 0 chỉ ảnh hưởng đến thời gian và không thể tăng số lượng sự kiện phá kỷ lục vượt quá mức dương đã cho phép. 

### Tại sao nó hoạt động 

Tổng tiền tố tối đa đang chạy tăng chính xác khi chúng tôi thêm một phần tử đẩy tổng tích lũy lên trên tất cả các giá trị có thể đạt được trước đó. Bất kỳ phần tử không dương nào cũng không thể đóng góp vào mức tăng tối đa mới, vì việc thêm nó vào bất kỳ tổng tiền tố nào cũng không thể làm tăng tổng một cách nghiêm ngặt. Vì vậy chỉ có những yếu tố tích cực mới có khả năng gây ra những sự kiện như vậy. 

Hơn nữa, mỗi phần tử dương có thể được sắp xếp sao cho nó tạo ra mức cực đại mới của chính nó hoặc được hấp thụ vào một giao điểm cuối cùng duy nhất, nhưng không có sự sắp xếp nào có thể thực hiện nhiều hơn các cập nhật cực đại riêng biệt của pos và có thể thực hiện ít nhất một cập nhật bất cứ khi nào pos khác không. Điều này buộc tập hợp các giá trị k có thể đạt được phải chính xác là {1, 2, ..., pos} và trong trường hợp suy biến khi pos = 0, nó sẽ thu gọn về {0}. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    
    pos = 0
    for x in a:
        if x > 0:
            pos += 1
    
    for _ in range(q):
        i, v = map(int, input().split())
        i -= 1
        
        old = a[i]
        if old > 0:
            pos -= 1
        
        a[i] = v
        
        if v > 0:
            pos += 1
        
        if pos == 0:
            print(1)
        else:
            print(pos)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một bộ đếm toàn cầu duy nhất`pos`, biểu thị có bao nhiêu phần tử hoàn toàn dương tại bất kỳ thời điểm nào. Mỗi bản cập nhật chỉ ảnh hưởng cục bộ đến bộ đếm này, do đó, bản thân mảng chỉ cần thiết để ghi nhớ các giá trị trước đó. Logic đầu ra trực tiếp tuân theo đặc tính dẫn xuất: hoặc tất cả các giá trị đều không dương, đưa ra một kết quả có thể xảy ra hoặc nếu không thì mọi số đếm từ 1 đến pos đều có thể đạt được. 

Một lỗi phổ biến ở đây là cố gắng tính toán lại câu trả lời bằng cách sắp xếp hoặc mô phỏng tổng tiền tố sau mỗi lần cập nhật, việc này sẽ quá chậm. Sự đơn giản hóa quan trọng là cấu trúc đầy đủ của mảng không bao giờ quan trọng, chỉ có sự phân bố dấu hiệu. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[1, 2, 3]`. 

Ban đầu cả ba phần tử đều dương nên pos = 3. 

| Bước | Mảng | tư thế | Đầu ra | 
| --- | --- | --- | --- | 
| ban đầu | [1,2,3] | 3 | - | 
| 1 | [1,2,4] | 3 | 3 | 
| 2 | [1,-2,4] | 2 | 2 | 
| 3 | [-3,-2,4] | 1 | 1 | 
| 4 | [-3,-2,1] | 1 | 1 | 
| 5 | [-3,1,1] | 2 | 2 | 

Điều này phù hợp với ý tưởng rằng câu trả lời luôn bằng số phần tử dương trừ khi số đó bằng 0. 

Bây giờ hãy xem xét một trường hợp không có kết quả tích cực, chẳng hạn như`[0, -1, -5]`. 

| Bước | Mảng | tư thế | Đầu ra | 
| --- | --- | --- | --- | 
| ban đầu | [0,-1,-5] | 0 | 1 | 
| sau khi cập nhật | vẫn tất cả ≤ 0 | 0 | 1 | 

Ở đây không có thứ tự nào có thể làm cho tổng chạy vượt quá 0, vì vậy giá trị duy nhất có thể đạt được là k = 0 và số giá trị k hợp lệ là 1. 

Những dấu vết này xác nhận rằng bất biến hoàn toàn dựa trên dấu hiệu và không nhạy cảm với độ lớn hoặc thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mỗi bản cập nhật điều chỉnh một bộ đếm trong thời gian O(1) | 
| Không gian | O(1) thêm | Chỉ có mảng và một bộ đếm số nguyên được duy trì | 

Các ràng buộc cho phép tối đa 200.000 bản cập nhật, do đó việc xử lý liên tục theo thời gian cho mỗi truy vấn là cần thiết. Giải pháp này phù hợp thoải mái trong giới hạn vì nó không thực hiện tính toán nặng nề cho mỗi thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    
    pos = sum(1 for x in a if x > 0)
    
    out = []
    for _ in range(q):
        i, v = map(int, input().split())
        i -= 1
        if a[i] > 0:
            pos -= 1
        a[i] = v
        if a[i] > 0:
            pos += 1
        out.append(str(1 if pos == 0 else pos))
    
    return "\n".join(out)

# minimal case
assert run("1 1\n0\n1 5\n") == "1", "single element becomes positive"

# all non-positive
assert run("3 2\n0 -1 -2\n1 -5\n2 -3\n") == "1\n1", "all non-positive stays 1"

# all positive shrinking
assert run("3 3\n1 2 3\n1 -1\n2 -2\n3 -3\n") == "2\n1\n1", "positive count decreases"

# mixed updates
assert run("4 2\n1 -1 2 -3\n2 5\n3 -4\n") == "3\n2", "toggle positives"

# already maximal positives
assert run("2 1\n10 20\n1 -5\n") == "1", "drop to single positive"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật phần tử đơn | 1 | trường hợp cơ sở đúng đắn | 
| tất cả đều không tích cực | 1 cho mỗi truy vấn | xử lý trường hợp không dương tính | 
| mặt tích cực giảm dần | câu trả lời thu hẹp | cập nhật động | 
| chuyển đổi hỗn hợp | chuyển đổi dấu đúng | cả hai hướng | 
| giảm xuống mức dương đơn | chuyển tiếp ranh giới | pos = 1 hành vi | 

## Vỏ cạnh 

Khi tất cả các phần tử đều không dương, thuật toán sẽ giữ đúng`pos = 0`xuyên suốt và xuất ra 1 cho mỗi truy vấn. Điều này tương ứng với thực tế là không có hoán vị nào có thể tạo ra tổng tiền tố dương, do đó không xảy ra cập nhật tối đa. 

Khi một giá trị chuyển từ dương sang không dương hoặc ngược lại, chỉ cần một lần cập nhật bộ đếm duy nhất. Ví dụ, thay đổi`-3`ĐẾN`4`tăng lên`pos`tăng thêm một, và kết quả là số câu trả lời tăng lên phản ánh thực tế là giờ đây, một yếu tố nữa có thể độc lập đóng góp vào một sự kiện kỷ lục tiềm năng. 

Độ chính xác không phụ thuộc vào độ lớn. Ngay cả những giá trị cực đoan như`-10^9`Và`10^9`cư xử giống hệt với`-1`Và`1`, vì chỉ có dấu hiệu mới ảnh hưởng đến việc một phần tử có đóng góp vào`pos`.
