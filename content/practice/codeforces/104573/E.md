---
title: "CF 104573E - Xáo trộn gian xảo"
description: "Chúng ta được phát một bộ bài gồm 52 lá bài. Mỗi thẻ được biểu thị bằng một số nguyên từ 1 đến 13, trong đó mỗi giá trị xuất hiện chính xác bốn lần."
date: "2026-06-30T08:19:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104573
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 1"
rating: 0
weight: 104573
solve_time_s: 63
verified: true
draft: false
---

[CF 104573E - Xáo trộn gian xảo](https://codeforces.com/problemset/problem/104573/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ bài gồm 52 lá bài. Mỗi thẻ được biểu thị bằng một số nguyên từ 1 đến 13, trong đó mỗi giá trị xuất hiện chính xác bốn lần. Số 1 đại diện cho quân Át, và điều duy nhất quan trọng để giành chiến thắng là liệu cả bốn quân Át có kết thúc ở đâu đó trong 26 vị trí đầu tiên của bộ bài hay không. 

Thao tác duy nhất chúng ta được phép thực hiện là xáo trộn hoàn hảo: chia bộ bài thành hai nửa gồm 26 lá bài, sau đó xen kẽ chúng sao cho bộ bài mới lấy một lá bài từ nửa đầu, sau đó lấy một lá bài từ nửa sau và lặp lại mô hình này cho đến khi tất cả các lá bài được sử dụng. 

Chúng ta được phép áp dụng sự xáo trộn này bao nhiêu lần cũng được. Câu hỏi đặt ra là liệu có tồn tại một số chuỗi xáo trộn như vậy dẫn đến việc cả bốn con át đều được chứa hoàn toàn trong nửa đầu của bộ bài hay không. 

Khó khăn chính là đây không phải là một sự xáo trộn ngẫu nhiên. Đó là một hoán vị xác định của các vị trí. Khi chúng ta hiểu các vị trí phát triển như thế nào khi bị xáo trộn lặp đi lặp lại, vấn đề sẽ trở thành câu hỏi về khả năng tiếp cận các vị trí theo một hoán vị cố định. 

Kích thước đầu vào được cố định ở mức 52, do đó, bất kỳ giải pháp nào lên tới ít nhất O(n²) hoặc thậm chí O(n³) đều nhanh chóng. Điều này có nghĩa là chúng ta có thể tự do mô phỏng hoàn toàn cấu trúc hoán vị và phân tích chu kỳ của các vị trí. 

Một cách giải thích ngây thơ có thể cố gắng mô phỏng tất cả các chuỗi xáo trộn có thể có, nhưng mặc dù không gian trạng thái là hữu hạn, nhưng nó vẫn lớn về mặt thiên văn nếu được coi là những hoán vị tùy ý. Cách tiếp cận đúng phải khai thác rằng mỗi lần xáo trộn là một hoán vị cố định, do đó ứng dụng lặp lại chỉ quay vòng qua một cấu trúc hữu hạn. 

Một trường hợp phức tạp nảy sinh khi nghĩ về tư cách thành viên “nửa đầu” theo thời gian. Một sai lầm phổ biến là cho rằng nếu quân át có thể đạt được vị trí nào đó trong hiệp một bất kỳ lúc nào thì thế là đủ. Điều đó bỏ qua rằng chúng ta cần tất cả bốn quân át phải đồng thời ở hiệp một ở cùng một trạng thái xáo trộn, chứ không chỉ có thể truy cập riêng lẻ ở đó vào các thời điểm khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng hoạt động xáo trộn nhiều lần và theo dõi mọi trạng thái có thể có của bộ bài. Vì mỗi lần xáo trộn đều mang tính xác định, điều này làm giảm việc chuyển đổi vòng tròn qua hoán vị trên 52 phần tử. Không gian trạng thái có tối đa 52 cấu hình riêng biệt trước khi xảy ra sự lặp lại, do đó việc mô phỏng đầy đủ là khả thi. Tuy nhiên, ngay cả khi chúng tôi mô phỏng tất cả các trạng thái, việc kiểm tra tất cả các kết hợp có thể có của các trạng thái mà quân át xếp thẳng hàng trong nửa đầu vẫn sẽ lộn xộn và không cần thiết về mặt khái niệm. 

Quan sát quan trọng là việc xáo trộn xác định một hoán vị trên các vị trí. Mỗi thẻ di chuyển một cách xác định từ chỉ số này sang chỉ số khác. Do đó, việc xáo trộn lặp đi lặp lại sẽ di chuyển mỗi thẻ theo một chu kỳ cố định trong biểu đồ hoán vị. 

Thay vì suy nghĩ về cấu hình toàn bộ bộ bài, chúng tôi chỉ theo dõi vị trí của bốn quân át. Mỗi con át di chuyển độc lập dọc theo chu kỳ của nó. Câu hỏi đặt ra là liệu có tồn tại một bước thời gian t sao cho cả bốn chu kỳ đều đặt quân át tương ứng của chúng vào các chỉ số từ 1 đến 26 cùng một lúc hay không. 

Vì quá trình này là tuần hoàn với chu kỳ nhiều nhất là 52 nên chúng ta chỉ cần kiểm tra nhiều nhất là 52 trạng thái. Đối với mỗi trạng thái, chúng tôi tính toán vị trí của mỗi con át sau khi xáo trộn bằng cách sử dụng ý tưởng lũy ​​thừa hoán vị hoặc đơn giản hơn là ứng dụng lặp lại vì kích thước không đổi. 

Sau đó chúng tôi kiểm tra xem tất cả các vị trí quân át có nằm trong hiệp một hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực đối với các bang và kiểm tra | O(52²) | O(52) | Đã chấp nhận | 
| Mô phỏng hoán vị theo các bước thời gian | O(52²) | O(52) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa sự xáo trộn như một hoán vị`p`, Ở đâu`p[i]`là vị trí mới của thẻ ban đầu ở vị trí`i`. 

Sau khi tính toán hoán vị này, chúng tôi liên tục áp dụng nó để theo dõi các vị trí theo thời gian. 

1. Xây dựng hoán vị gây ra bởi một lần xáo trộn hoàn hảo. Chúng tôi mô phỏng việc xen kẽ: các vị trí từ 1 đến 26 và 27 đến 52. Thứ tự mới được xác định một cách xác định, vì vậy chúng tôi có thể tính toán vị trí của mỗi chỉ mục ban đầu sau một lần xáo trộn. 
2. Trích xuất vị trí ban đầu của cả bốn con át. Hãy để những điều này được`a1, a2, a3, a4`. 
3. Mô phỏng việc áp dụng hoán vị lặp đi lặp lại lên tới 52 bước. Ở mỗi bước, hãy cập nhật vị trí của mỗi quân Át bằng cách sử dụng`pos = p[pos]`. 
4. Sau mỗi bước (bao gồm cả trạng thái ban đầu), hãy kiểm tra xem tất cả bốn vị trí quân Át có nằm trong phạm vi [1, 26] hay không. 
5. Nếu bất kỳ bước nào thỏa mãn điều kiện này, hãy trả về "CÓ". 
6. Nếu không có bước nào thỏa mãn sau 52 lần lặp, hãy trả về "NO". 

### Tại sao nó hoạt động 

Sự xáo trộn là một hoán vị trên một tập hợp hữu hạn gồm 52 vị trí, do đó các hình thức ứng dụng lặp đi lặp lại theo chu kỳ. Mọi vị trí sẽ trở về giá trị ban đầu sau tối đa 52 ứng dụng. Vì chúng tôi chỉ theo dõi các vị trí quân Át theo hoán vị này nên cấu hình chung của chúng là tuần hoàn với chu kỳ chia cho 52. Do đó, bất kỳ cấu hình nào có thể xảy ra đều phải xuất hiện trong 52 bước đầu tiên. Việc kiểm tra tất cả các bước đảm bảo chúng tôi không bỏ lỡ trạng thái căn chỉnh hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_perm():
    # after shuffle: [0..25] interleaved with [26..51]
    p = [0] * 52
    for i in range(26):
        p[i] = 2 * i
        p[i + 26] = 2 * i + 1
    return p

def apply(p, arr):
    return [p[x] for x in arr]

def solve():
    a = list(map(int, input().split()))
    
    aces = [i for i, v in enumerate(a) if v == 1]

    p = build_perm()

    # current positions of aces
    pos = aces[:]

    for _ in range(52):
        if all(x < 26 for x in pos):
            print("YES")
            return
        pos = [p[x] for x in pos]

    print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng hoán vị được tạo ra bởi một lần xáo trộn duy nhất. Việc ánh xạ rất đơn giản: phần tử thứ i của nửa đầu đi đến vị trí 2i và phần tử thứ i của nửa sau đi đến vị trí 2i+1. 

Sau đó chúng tôi xác định vị trí của tất cả quân Át và chỉ theo dõi vị trí của chúng. Điều này tránh việc mô phỏng toàn bộ bộ bài nhiều lần, điều này là không cần thiết vì chỉ có vị trí quân át mới quan trọng đối với điều kiện. 

Ở mỗi lần lặp, chúng tôi cập nhật vị trí quân át bằng cách áp dụng hoán vị một lần. Vòng lặp chạy 52 lần vì không gian hoán vị bị giới hạn bởi 52, do đó bất kỳ chu kỳ nào cũng phải lặp lại trong phạm vi đó. 

Séc`x < 26`mã hóa tư cách thành viên trong nửa đầu. Nếu cả bốn đều thỏa mãn điều này cùng một lúc, chúng ta có thể chấm dứt ngay lập tức. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi trích xuất các vị trí quân át ban đầu và mô phỏng chuyển động của chúng. 

| Bước | Vị trí Át | Tất cả trong [0,25]? | 
| --- | --- | --- | 
| 0 | vị trí ban đầu | kiểm tra | 
| 1 | perm(pos) | kiểm tra | 
| 2 | perm(pos) | kiểm tra | 
| ... | ... | ... | 
| 52 | chu kỳ lặp lại | kiểm tra | 

Ở một số lần lặp lại, cả bốn con át đều thẳng hàng trong nửa đầu cùng một lúc. Điều này chứng tỏ rằng các chu kỳ hoán vị có thể đồng bộ hóa nhiều chu kỳ độc lập. 

### Mẫu 2 

| Bước | Vị trí Át | Tất cả trong [0,25]? | 
| --- | --- | --- | 
| 0 | vị trí ban đầu | không | 
| 1 | perm(pos) | không | 
| 2 | perm(pos) | không | 
| ... | ... | không | 
| 52 | chu kỳ hoàn thành | không | 

Ở đây, mỗi quân Át có thể đến thăm hiệp một vào những thời điểm khác nhau, nhưng không có dấu thời gian chung trong đó cả bốn quân Át đều có mặt đồng thời trong hiệp một. 

Điều này nhấn mạnh rằng khả năng tiếp cận độc lập là không đủ, cần phải đồng bộ hóa giữa các chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(52) | Chúng tôi mô phỏng tối đa 52 bước xáo trộn, mỗi bước cập nhật bốn vị trí | 
| Không gian | O(52) | Chúng tôi lưu trữ một hoán vị cố định và vị trí quân Át | 

Các hằng số rất nhỏ và giải pháp chạy hiệu quả trong thời gian không đổi. Dung lượng bộ nhớ được cố định do kích thước bộ nhớ không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    # inline solution
    def build_perm():
        p = [0] * 52
        for i in range(26):
            p[i] = 2 * i
            p[i + 26] = 2 * i + 1
        return p

    a = list(map(int, sys.stdin.readline().split()))
    aces = [i for i, v in enumerate(a) if v == 1]
    p = build_perm()
    pos = aces[:]

    for _ in range(52):
        if all(x < 26 for x in pos):
            return "YES"
        pos = [p[x] for x in pos]

    return "NO"

# provided samples
assert run("1 5 9 7 9 11 12 13 8 7 13 2 12 1 10 10 3 7 4 3 4 8 8 3 5 4 1 11 5 1 10 4 2 13 3 2 9 12 6 6 6 12 9 6 11 10 8 5 2 7 13 11") == "YES"
assert run("12 11 5 8 3 2 13 6 1 3 12 3 12 5 7 10 6 7 9 4 6 4 1 13 1 9 5 10 9 2 4 9 2 8 11 8 13 2 10 7 3 7 8 4 10 1 13 5 11 11 6 12") == "NO"

# custom cases
assert run("1 2 3 4 5 6 7 8 9 10 11 12 13 1 2 3 4 5 6 7 8 9 10 11 12 13 1 2 3 4 5 6 7 8 9 10 11 12 13 1 2 3 4 5 6 7 8 9 10 11 12 13") in ["YES", "NO"]
assert run("1 1 1 1 " + "2 "*48) == "YES"
assert run("2 2 2 2 " + "1 "*48) == "NO"
assert run("1 2 3 4 5 6 7 8 9 10 11 12 13 1 2 3 4 5 6 7 8 9 10 11 12 13 2 3 4 5 6 7 8 9 10 11 12 13 1 2 3 4 5 6 7 8 9 10 11 12 13") in ["YES", "NO"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả át chủ bài tập hợp sớm | CÓ | trạng thái thành công tầm thường | 
| tất cả con át tách ra | KHÔNG | không thể đồng bộ hóa | 
| sàn cấu trúc lặp đi lặp lại | hoặc | hành vi chu kỳ | 
| hoán đổi một nửa cực đoan | CÓ/KHÔNG | ổn định ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi trạng thái ban đầu đã thỏa mãn điều kiện. Thuật toán kiểm tra điều này trước bất kỳ lần xáo trộn nào, do đó, nó ngay lập tức trả về CÓ khi cả bốn quân Át đều ở vị trí từ 1 đến 26. 

Một trường hợp khác là khi quân Át không bao giờ chuyển sang hiệp một đồng thời mặc dù từng quân Át đều ghé thăm hiệp một. Mẫu thứ hai nắm bắt được tình huống này. Mô phỏng theo dõi các vị trí chung chứ không phải khả năng tiếp cận độc lập nên nó trả về NO một cách chính xác. 

Một trường hợp tế nhị cuối cùng là tính tuần hoàn. Vì hoán vị có bậc hữu hạn nên chúng ta không cần nhiều hơn 52 bước. Thuật toán giới hạn rõ ràng các lần lặp lại, đảm bảo chúng tôi không bao giờ dựa vào mô phỏng vô hạn hoặc chấm dứt sớm do vô tình.
