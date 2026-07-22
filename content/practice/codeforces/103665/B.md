---
title: "CF 103665B - \u041f\u0435\u0440\u0435\u0432\u043e\u0434\u0447\u0438\u043a"
description: "Chúng ta được cung cấp một từ thuộc đúng một trong hai bảng chữ cái của người ngoài hành tinh. Một bảng chữ cái chỉ sử dụng các chữ cái A và B, trong khi bảng chữ cái kia chỉ sử dụng các chữ số 0 và 1."
date: "2026-07-03T02:06:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103665
codeforces_index: "B"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2018"
rating: 0
weight: 103665
solve_time_s: 43
verified: true
draft: false
---

[CF 103665B - \u041f\u0435\u0440\u0435\u0432\u043e\u0434\u0447\u0438\u043a](https://codeforces.com/problemset/problem/103665/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một từ thuộc đúng một trong hai bảng chữ cái của người ngoài hành tinh. Một bảng chữ cái chỉ sử dụng các chữ cái`A`Và`B`, trong khi cái kia chỉ sử dụng các chữ số`0`Và`1`. Nhiệm vụ là xác định từ đầu vào thuộc bảng chữ cái nào, sau đó chuyển đổi nó sang bảng chữ cái khác bằng cách sử dụng ánh xạ từng ký tự cố định. 

Việc lập bản đồ là hoàn toàn xác định. Mọi`A`trở thành`0`, mọi`B`trở thành`1`, và ngược lại mọi`0`trở thành`A`, mọi`1`trở thành`B`. Không có cấu trúc nào ngoài việc thay thế ký tự độc lập, do đó độ dài từ không thay đổi và mỗi vị trí có thể được xử lý mà không cần ngữ cảnh. 

Giới hạn kích thước đầu vào nhỏ, với độ dài từ lên tới 100 ký tự. Điều này ngay lập tức ngụ ý rằng ngay cả việc quét tuyến tính đơn giản nhất cũng nhanh không đáng kể, vì chúng tôi thực hiện tối đa 100 lần kiểm tra và thay thế ký tự. Bất cứ điều gì lên tới O(n²) vẫn ổn, nhưng không có lý do gì để vượt quá O(n). 

Các trường hợp cạnh là tối thiểu, nhưng có một số trường hợp tinh tế đáng được nêu rõ ràng. Độ dài từ có thể là 1, ví dụ đầu vào`A`, cái này sẽ trở thành`0`. Tương tự,`1`nên trở thành`B`. Một trường hợp khác là khi toàn bộ chuỗi đã ở dạng nhị phân, chẳng hạn như`0101`, phải được chuyển đổi hoàn toàn thành chữ cái. Việc triển khai bất cẩn có thể giả định không chính xác một bảng chữ cái mà không kiểm tra, nhưng đảm bảo cho biết đầu vào hoàn toàn thuộc về một bảng chữ cái, vì vậy chỉ cần quét một lần là đủ để quyết định. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ coi đây là một vấn đề dịch thuật chung: trước tiên hãy thử phát hiện bảng chữ cái, sau đó liên tục áp dụng các thay thế hoặc xây dựng các chuỗi trung gian trong khi quét nhiều lần. Người ta có thể tưởng tượng việc thay thế các ký tự bằng cách sử dụng các thao tác chuỗi lặp lại hoặc sử dụng các vòng lặp lồng nhau liên tục quét và thay thế các ký tự cho đến khi không còn thay đổi nào. Điều này vẫn có hiệu quả vì việc chuyển đổi đơn giản và cục bộ nhưng lại tốn chi phí không cần thiết. 

Quan sát chính là đầu vào được đảm bảo đồng nhất. Hoặc tất cả các nhân vật đều đến từ`{A, B}`hoặc tất cả đều đến từ`{0, 1}`. Điều đó có nghĩa là chúng tôi không cần bất kỳ logic xác thực hoặc phát hiện phức tạp nào ngoài việc kiểm tra các ký tự khi chúng tôi quét. Một lần duyệt là đủ: chúng ta có thể ánh xạ trực tiếp từng ký tự tới ký tự đối tác của nó và xây dựng kết quả sau một lần quét. 

Điều này làm giảm vấn đề thành vấn đề thay thế ký tự trực tiếp, trong đó mỗi vị trí được chuyển đổi độc lập trong O(1), đưa ra giải pháp tổng thể O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (quét/xây dựng lại nhiều lần) | O(n²) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Ánh xạ một lượt tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, cho chúng ta biết có bao nhiêu ký tự trong từ. Chúng tôi thực sự không cần nó để xử lý nhưng nó đảm bảo tính nhất quán đầu vào. 
2. Đọc chuỗi`s`chiều dài`n`. 
3. Kiểm tra ký tự đầu tiên của chuỗi để xác định hướng dịch. Nếu nó là`A`hoặc`B`, thì chúng ta đang dịch từ bảng chữ cái sang hệ nhị phân. Nếu không thì phải`0`hoặc`1`, nghĩa là chúng tôi dịch từ nhị phân sang chữ cái. Điều này hoạt động vì sự cố đảm bảo chuỗi hoàn toàn nhất quán. 
4. Tạo danh sách trống hoặc vùng đệm để tạo đầu ra một cách hiệu quả. Việc sử dụng danh sách sẽ tránh việc nối chuỗi lặp lại, việc này sẽ chậm hơn trong Python. 
5. Lặp lại từng ký tự trong chuỗi. Đối với mỗi ký tự, áp dụng ánh xạ: 

Nếu chúng ta đang ở chế độ chuyển chữ cái sang nhị phân, hãy thay thế`A → 0`Và`B → 1`. 

Nếu chúng ta đang ở chế độ chuyển đổi nhị phân thành chữ cái, hãy thay thế`0 → A`Và`1 → B`. 

Nối ký tự đã chuyển đổi vào bộ đệm đầu ra. 
6. Nối bộ đệm thành chuỗi cuối cùng và in nó. 

### Tại sao nó hoạt động 

Mỗi ký tự độc lập với tất cả các ký tự khác và quy tắc dịch là song ngữ giữa hai bảng chữ cái. Điều này có nghĩa là mọi chuỗi đầu vào hợp lệ sẽ ánh xạ tới chính xác một chuỗi đầu ra hợp lệ mà không có sự mơ hồ. Vì chúng tôi xác định chế độ một lần và áp dụng phép biến đổi cố định cho mỗi ký tự, thuật toán duy trì vị trí chính xác theo từng vị trí. Không có nhân vật nào phụ thuộc vào ngữ cảnh, do đó không có cơ hội xảy ra lỗi xếp tầng hoặc tham nhũng trạng thái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    # detect alphabet from first character
    if s[0] in "AB":
        # Dobos -> Femos
        mp = {'A': '0', 'B': '1'}
    else:
        # Femos -> Dobos
        mp = {'0': 'A', '1': 'B'}

    res = []
    for c in s:
        res.append(mp[c])

    print("".join(res))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo thuật toán chính xác. Từ điển`mp`mã hóa quy tắc dịch, vì vậy mỗi lần tra cứu ký tự là thời gian không đổi. Kết quả được tích lũy trong một danh sách để tránh hành vi bậc hai khi nối chuỗi. Quyết định về hướng được đưa ra một lần dựa trên ký tự đầu tiên, điều này an toàn do đảm bảo rằng toàn bộ chuỗi thuộc về một bảng chữ cái. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
s = ABAAB
```Đầu tiên chúng tôi phát hiện ra điều đó`A`là một chữ cái nên chúng ta đang dịch các chữ cái sang dạng nhị phân. 

| Bước | Nhân vật | Lập bản đồ | Đầu ra cho đến nay | 
| --- | --- | --- | --- | 
| 1 | A | 0 | 0 | 
| 2 | B | 1 | 01 | 
| 3 | A | 0 | 010 | 
| 4 | A | 0 | 0100 | 
| 5 | B | 1 | 01001 | 

Đầu ra:```
01001
```Điều này xác nhận rằng mỗi ký tự được ánh xạ độc lập và không cần chuyển đổi cấu trúc. 

### Ví dụ 2 

đầu vào:```
n = 2
s = 11
```Chúng tôi phát hiện các chữ số nên chúng tôi dịch nhị phân sang chữ cái. 

| Bước | Nhân vật | Lập bản đồ | Đầu ra cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 1 | B | B | 
| 2 | 1 | B | BB | 

Đầu ra:```
BB
```Điều này cho thấy hướng ngược lại hoạt động đối xứng và sử dụng cùng một logic cho mỗi ký tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với tra cứu O(1) | 
| Không gian | O(n) | Chuỗi đầu ra được lưu trữ rõ ràng | 

Ràng buộc n 100 làm cho độ phức tạp này thấp hơn nhiều so với bất kỳ giới hạn thực tế nào. Ngay cả với chi phí sử dụng I/O Python, giải pháp vẫn chạy ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    n = int(input().strip())
    s = input().strip()

    if s[0] in "AB":
        mp = {'A': '0', 'B': '1'}
    else:
        mp = {'0': 'A', '1': 'B'}

    return "".join(mp[c] for c in s)

# provided samples
assert run("5\nABAAB\n") == "01001"
assert run("2\n11\n") == "BB"

# custom cases
assert run("1\nA\n") == "0"
assert run("1\n1\n") == "B"
assert run("4\nABAB\n") == "0101"
assert run("4\n0101\n") == "ABAB"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 A`|`0`| độ dài tối thiểu, chữ cái sang nhị phân | 
|`1 1`|`B`| độ dài tối thiểu, nhị phân sang chữ cái | 
|`ABAB`|`0101`| tính nhất quán của mô hình xen kẽ | 
|`0101`|`ABAB`| tính nhất quán của ánh xạ ngược | 

## Vỏ cạnh 

Đối với đầu vào một ký tự, thuật toán vẫn xác định chính xác bảng chữ cái từ ký tự đó và áp dụng ánh xạ một lần. Ví dụ, đầu vào`A`thiết lập bản đồ`{A → 0}`và vòng lặp chạy khi tạo ra`0`. 

Đối với đầu vào ký tự đơn nhị phân`1`, ánh xạ là`{1 → B}`, và đầu ra là`B`. Không có sự mơ hồ vì bước phát hiện chỉ dựa vào tư cách thành viên trong các bảng chữ cái cố định và các bảng chữ cái đó rời rạc. 

Đối với các chuỗi xen kẽ như`ABAB`, thuật toán không giả định cấu trúc, nó chỉ áp dụng ánh xạ cho mỗi chỉ mục. Mỗi vị trí là độc lập nên không có sự tương tác giữa các vị trí ảnh hưởng đến tính chính xác.
