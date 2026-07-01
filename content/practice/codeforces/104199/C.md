---
title: "CF 104199C - \u0411\u0435\u0437\u043b\u044e\u0434\u043d\u044b\u0439 \u043e\u0442\u0435\u043b\u044c"
description: "Chúng ta được cung cấp một từ được viết dưới dạng một chuỗi các chữ cái Latinh viết thường. Mã thông báo bắt đầu từ ký tự đầu tiên và di chuyển theo quy tắc xác định phụ thuộc hoàn toàn vào số lần ký tự hiện tại xuất hiện trong từ."
date: "2026-07-02T00:01:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104199
codeforces_index: "C"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 18-02-23"
rating: 0
weight: 104199
solve_time_s: 84
verified: true
draft: false
---

[CF 104199C - \u0411\u0435\u0437\u043b\u044e\u0434\u043d\u044b\u0439 \u043e\u0442\u0435\u043b\u044c](https://codeforces.com/problemset/problem/104199/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một từ được viết dưới dạng một chuỗi các chữ cái Latinh viết thường. Mã thông báo bắt đầu từ ký tự đầu tiên và di chuyển theo quy tắc xác định phụ thuộc hoàn toàn vào số lần ký tự hiện tại xuất hiện trong từ. 

Nếu ký tự bên dưới mã thông báo xuất hiện nhiều lần trong chuỗi, mã thông báo được phép chuyển sang bất kỳ lần xuất hiện nào khác của cùng một ký tự. Nếu ký tự xuất hiện đúng một lần, mã thông báo buộc phải di chuyển một bước sang phải. Nếu nó đã ở vị trí cuối cùng và ký tự là duy nhất thì quá trình sẽ kết thúc. 

Câu hỏi đặt ra là liệu quá trình này luôn kết thúc hay liệu nó có thể tiếp tục mãi mãi bằng cách chọn bước nhảy giữa các ký tự bằng nhau hay không. 

Ràng buộc n lên tới 100000 ngụ ý rằng bất kỳ mô phỏng nào khám phá quá trình chuyển đổi từng bước đều không an toàn nếu nó có thể quay vòng qua các trạng thái nhiều lần. Quá trình này có thể xem lại các vị trí nhiều lần vì các chữ cái lặp lại cho phép nhảy tùy ý, do đó, không gian trạng thái thực sự là một biểu đồ có tới 100000 nút và nhiều cạnh được tạo ra bởi các ký tự giống hệt nhau. Một mô phỏng đơn giản chạy cho đến khi kết thúc hoặc phát hiện chu kỳ có thể giảm xuống hành vi bậc hai trong trường hợp xấu nhất do phải xem lại nhiều lần. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các ký tự giống hệt nhau. Ví dụ: trong một chuỗi như "aaa", mã thông báo không bao giờ tiến lên một cách xác định và luôn có thể nhảy qua lại, tạo ra một vòng lặp vô hạn. Mặt khác, trong một chuỗi như "abc", mỗi ký tự là duy nhất nên mã thông báo luôn di chuyển sang phải và kết thúc ngay lập tức. 

Khó khăn chính là nhận biết khi nào cấu trúc nhảy tạo ra một chu trình thay vì thực sự mô phỏng quá trình. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực coi mỗi vị trí là một trạng thái. Từ vị trí i, nếu ký tự hiện tại xuất hiện nhiều lần, chúng tôi xem xét chuyển đổi sang tất cả các chỉ mục khác có cùng ký tự. Ngược lại, chúng ta chuyển đến i + 1. Chúng ta tiếp tục quá trình này cho đến khi chúng ta rời khỏi chuỗi hoặc quay lại vị trí đã thấy trước đó trong cấu hình đảm bảo sự lặp lại. 

Điều này hoạt động về mặt khái niệm vì quy trình này mang tính quyết định khi chúng tôi sửa một lựa chọn bước nhảy, nhưng vấn đề cho phép lựa chọn tùy ý giữa các chữ cái bằng nhau, có nghĩa là quy trình không phải là một đường dẫn duy nhất mà là một hệ thống phân nhánh. Một mô phỏng trực tiếp sẽ cần khám phá tất cả các lựa chọn có thể có hoặc duy trì một tập hợp đã truy cập trên một biểu đồ trạng thái ẩn. Trong trường hợp xấu nhất, mọi vị trí có thể kết nối với nhiều vị trí khác có cùng đặc điểm và việc truy cập lại nhiều lần có thể tạo ra sự bùng nổ theo cấp số nhân hoặc bậc hai. 

Quan sát quan trọng là cách duy nhất để đạt được tiến bộ là thông qua các ký tự xuất hiện chính xác một lần trong toàn bộ hành vi hậu tố của quy trình. Nếu tại bất kỳ thời điểm nào, mã thông báo nằm trên một ký tự xuất hiện nhiều lần, chúng ta luôn có thể chọn chuyển sang lần xuất hiện trước đó và tạo lại tình huống đã thấy trước đó. Khả năng quay trở lại trạng thái trước đó ngụ ý rằng hệ thống có tính tuần hoàn trừ khi tiến trình bắt buộc chiếm ưu thế. 

Do đó, vấn đề giảm xuống còn việc kiểm tra xem liệu quy trình có thể đạt đến điểm mà mọi ký tự mà chúng tôi truy cập là duy nhất trong cấu trúc có thể truy cập còn lại theo cách buộc chuyển động sang phải mang tính quyết định cho đến khi kết thúc hay không. Điều này sụp đổ thành điều kiện cấu trúc theo tần số: nếu tồn tại bất kỳ ký tự nào có tần số ít nhất là 2, thì luôn có thể tạo một vòng lặp bằng cách nảy giữa các lần xuất hiện của ký tự đó, bởi vì không có quy tắc nào ngăn cản việc quay trở lại các chỉ mục trước đó vô thời hạn. 

Do đó, việc chấm dứt xảy ra khi và chỉ khi tất cả các ký tự khác nhau trong chuỗi.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng chuyển tiếp Brute | O(n²) trường hợp xấu nhất | O(n) | Quá chậm | 
| Kiểm tra tần suất | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp dựa trên việc đếm số lần xuất hiện của từng ký tự và kiểm tra xem có ký tự nào xuất hiện nhiều lần hay không. 

1. Đếm tần số của mỗi ký tự trong chuỗi. Điều này được thực hiện trong một lần chuyển qua đầu vào, duy trì một mảng cố định có kích thước 26 vì chỉ sử dụng các chữ cái Latinh viết thường. Bước này nắm bắt tất cả sự mơ hồ về cấu trúc trong quy trình. 
2. Quét qua bảng tần số và kiểm tra xem có ký tự nào có tần số lớn hơn 1 hay không. Sự hiện diện của ký tự đó có nghĩa là tồn tại ít nhất một vị trí mà mã thông báo luôn có thể chọn nhảy trở lại ký tự giống hệt khác. 
3. Nếu bất kỳ tần số nào vượt quá 1, hãy kết luận rằng quy trình có thể được thực hiện vô hạn và xuất ra NO. Ngược lại, nếu tất cả tần số đều bằng 1 thì mã thông báo luôn di chuyển sang phải cho đến khi thoát khỏi chuỗi, do đó xuất ra CÓ. 

### Tại sao nó hoạt động 

Quá trình chỉ trở nên không xác định ở các ký tự xuất hiện nhiều lần. Với ký tự như vậy, mã thông báo có quyền tự do chuyển sang vị trí giống hệt khác, vị trí này luôn duy trì khả năng truy cập lại các cấu hình trước đó. Vì các vị trí là hữu hạn nên việc xem lại nhiều lần dưới sự tự do này tạo thành một chu kỳ. Nếu không có ký tự nào lặp lại, mọi bước di chuyển sẽ buộc phải chuyển sang chỉ mục tiếp theo và trạng thái sẽ tiến triển nghiêm ngặt cho đến khi kết thúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    freq = [0] * 26
    for ch in s:
        freq[ord(ch) - ord('a')] += 1

    for c in freq:
        if c > 1:
            print("NO")
            return

    print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc độ dài và chuỗi. Mảng tần số theo dõi sự xuất hiện của từng ký tự trong không gian không đổi. Việc quét các bản sao là bước quyết định: nó không cố gắng mô phỏng chuyển động, bởi vì thuộc tính duy nhất quan trọng là liệu sự phân nhánh có tồn tại ở bất kỳ vị trí nào hay không. 

Séc`c > 1`là đủ vì ngay cả một ký tự lặp lại cũng đưa ra cơ chế chuyển đổi có thể đảo ngược, cho phép mã thông báo lặp vô thời hạn bằng cách xen kẽ giữa các lần xuất hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1: "letovo" 

Chúng tôi theo dõi tần số đầu tiên. 

| nhân vật | đếm | 
| --- | --- | 
| tôi | 1 | 
| e | 1 | 
| t | 1 | 
| o | 2 | 
| v | 1 | 

Vì 'o' xuất hiện hai lần nên thuật toán ngay lập tức quyết định rằng có thể thực hiện một chu trình và đưa ra NO. 

Điều này phù hợp với hành vi trực quan trong đó mã thông báo có thể nảy giữa hai vị trí 'o' vô thời hạn. 

### Ví dụ 2: "abc" 

| nhân vật | đếm | 
| --- | --- | 
| một | 1 | 
| b | 1 | 
| c | 1 | 

Tất cả số đếm đều là 1, vì vậy mã thông báo luôn di chuyển sang phải một cách xác định: 0 → 1 → 2 → thoát. Đầu ra là CÓ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần để tính toán tần số và quét bảng chữ cái | 
| Không gian | O(1) | mảng tần số có độ dài 26 cố định | 

Kích thước đầu vào lên tới 100000 có thể được xử lý dễ dàng do giải pháp chỉ thực hiện quét tuyến tính với bộ nhớ phụ không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("6\nletovo\n") == "NO"
assert run("3\nabc\n") == "YES"
assert run("3\naaa\n") == "NO"

# custom cases
assert run("1\na\n") == "YES"          # single character
assert run("2\nab\n") == "YES"         # minimal distinct
assert run("2\naa\n") == "NO"          # immediate repeat
assert run("5\nabcde\n") == "YES"      # all unique longer
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 một | CÓ | trường hợp nhỏ nhất | 
| aa | KHÔNG | lặp lại ngay lập tức | 
| abcde | CÓ | trường hợp riêng biệt chung | 
| aaa | KHÔNG | chu kỳ lặp lại đầy đủ | 

## Vỏ cạnh 

Chuỗi ký tự đơn tối thiểu như "a" bắt đầu bằng một ký tự duy nhất, do đó mã thông báo sẽ ngay lập tức di chuyển ra khỏi giới hạn và kết thúc. Việc kiểm tra tần số không thấy sự lặp lại và trả về CÓ. 

Một chuỗi như "aa" thể hiện tính không kết thúc ngay lập tức: cả hai vị trí đều có cùng một ký tự, do đó, mặc dù mã thông báo có thể di chuyển nhưng nó luôn có một lần xuất hiện thay thế để nhảy tới, cho phép một vòng lặp vô hạn giữa hai chỉ số. Thuật toán gắn cờ chính xác tần số 2 và trả về NO. 

Một chuỗi như "abcde" không chứa bản sao, vì vậy mỗi bước đều bị buộc phải sang phải mà không phân nhánh, đảm bảo kết thúc ở cuối chuỗi.
