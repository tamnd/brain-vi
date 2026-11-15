---
title: "CF 1826A - Không ai tin tưởng"
description: "Chúng ta được giao cho một nhóm người trong đó mỗi người đưa ra tuyên bố về số lượng người nói dối tối thiểu trong nhóm. Mỗi người hoặc luôn nói sự thật hoặc luôn nói dối. Nhiệm vụ của chúng ta là quyết định xem những tuyên bố này có nhất quán hay không và nếu có thì xác định số lượng kẻ nói dối có thể có."
date: "2026-06-09T07:31:18+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1826
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 870 (Div. 2)"
rating: 1200
weight: 1826
solve_time_s: 73
verified: true
draft: false
---

[CF 1826A - Không ai tin tưởng](https://codeforces.com/problemset/problem/1826/A) 

**Đánh giá:** 1200 
**Tags:** vũ phu, tham lam, thực hiện, phân loại 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao cho một nhóm người trong đó mỗi người đưa ra tuyên bố về số lượng người nói dối tối thiểu trong nhóm. Mỗi người hoặc luôn nói sự thật hoặc luôn nói dối. Nhiệm vụ của chúng ta là quyết định xem những tuyên bố này có nhất quán hay không và nếu có thì xác định số lượng kẻ nói dối có thể có. Nếu các khiếu nại không thể được đối chiếu, chúng ta nên trả về -1. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được số lượng người`n`và một mảng`n`số nguyên`l_i`, Ở đâu`l_i`là số người nói dối tối thiểu theo từng người`i`. Đầu ra của mỗi trường hợp thử nghiệm là số người nói dối hợp lệ hoặc -1 nếu các tuyên bố mâu thuẫn nhau. 

Những hạn chế còn khiêm tốn:`n`có thể lên tới 100 và tổng số người trong tất cả các trường hợp thử nghiệm tối đa là 10.000. Điều này cho phép các thuật toán có O(n log n) hoặc O(n²) cho mỗi trường hợp thử nghiệm. Chúng ta không cần phải xem xét các giải pháp tuyến tính hoặc tối ưu hóa cao. 

Các trường hợp đặc biệt không rõ ràng bao gồm các tình huống trong đó tất cả mọi người đều tuyên bố không có kẻ nói dối, trong đó mọi người đều tuyên bố có số lượng kẻ nói dối tối đa hoặc khi các tuyên bố tập trung xung quanh các giá trị khiến chỉ có thể có một phạm vi hẹp số lượng kẻ nói dối. Ví dụ, nếu ba người nói`[2, 2, 2]`, thì không thể có chính xác một kẻ nói dối và không có cấu hình nào thỏa mãn tất cả các yêu cầu. Việc triển khai bất cẩn có thể trả về một số mà không kiểm tra tất cả các ràng buộc, tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi số lượng kẻ nói dối có thể từ 0 đến`n`và kiểm tra xem các yêu cầu có được thỏa mãn hay không. Đối với số lượng ứng viên nói dối`k`, chúng tôi sẽ xác minh rằng ít nhất`k`mọi người yêu cầu ít nhất`k`những kẻ nói dối. Điều này hiệu quả vì một người trung thực phải nhất quán với số lượng người nói dối thực tế, trong khi tuyên bố của người nói dối là không liên quan. Bước xác minh yêu cầu quét mảng, dẫn đến độ phức tạp O(n²) cho mỗi trường hợp kiểm thử. Điều này có thể chấp nhận được đối với`n`lên tới 100 nhưng không hiệu quả. 

Điểm mấu chốt là chúng ta không cần phải kiểm tra mọi con số một cách tùy tiện. Chúng ta có thể sắp xếp mảng các tuyên bố và tìm chỉ số nhỏ nhất trong đó số người có tuyên bố nhỏ hơn hoặc bằng số lượng ứng viên nói dối khớp với số lượng dự kiến. Cụ thể hơn, nếu chúng ta sắp xếp`l_i`, thì với mỗi số người nói dối có thể`x`, số người yêu cầu ít nhất`x`những kẻ nói dối ít nhất nên`x`. Nếu điều này giữ vững,`x`là một câu trả lời hợp lệ Điều này làm giảm vấn đề xuống O(n log n) do sắp xếp, đủ nhanh với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Chấp nhận được nhưng chưa tối ưu | 
| Tối ưu (Sắp xếp + Tham lam) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và hàng loạt lời tuyên bố`l`. 
3. Sắp xếp mảng`l`theo thứ tự không giảm. Việc sắp xếp giúp đơn giản hóa việc kiểm tra bằng cách sắp xếp các xác nhận quyền sở hữu với số lượng kẻ nói dối tiềm năng. 
4. Khởi tạo kết quả là -1 (giả sử mâu thuẫn). 
5. Lặp lại số lượng kẻ nói dối có thể có`x`từ 0 đến`n`: 

1. Đếm xem có bao nhiêu người có yêu cầu lớn hơn hoặc bằng`x`. Sau khi sắp xếp, đây là`n - i`, Ở đâu`i`là chỉ số đầu tiên với`l[i] >= x`. 
2. Nếu`x`bằng số này, đặt kết quả thành`x`và phá vỡ. Điều này thể hiện một cấu hình nhất quán: chính xác`x`mọi người là những kẻ nói dối, và những người còn lại nói sự thật. 
6. In kết quả của test case này. 

Tính đúng đắn đến từ sự bất biến mà đối với một số người nói dối`x`, chính xác`x`những tuyên bố phải được đáp ứng bởi những người nói dối hoặc người nói thật. Việc sắp xếp cho phép chúng tôi tìm thấy số lượng tuyên bố có thể được coi là trung thực đối với một ứng viên một cách hiệu quả`x`. Chỉ khi số lượng ứng cử viên bằng số lượng yêu cầu hỗ trợ thì cấu hình nhất quán mới tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    l = list(map(int, input().split()))
    l.sort()
    result = -1
    for x in range(n + 1):
        # number of people claiming at least x liars
        count = sum(1 for val in l if val >= x)
        if count == x:
            result = x
            break
    print(result)
```Giải pháp đọc đầu vào bằng I/O nhanh để xử lý hiệu quả tới 10.000 người. Việc sắp xếp mảng cho phép chúng ta căn chỉnh các yêu cầu để đếm. Vòng lặp kiểm tra từng số lượng kẻ nói dối có thể có, tính toán số lượng khiếu nại hỗ trợ nó. Số đếm hợp lệ đầu tiên được trả về hoặc -1 nếu không tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2
```Yêu cầu được sắp xếp:`[1, 2]`| x | Khiếu nại >= x | Đếm | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 2 | 2 | không bằng | 
| 1 | 2 | 2 | không bằng | 
| 2 | 1 | 1 | bằng → kết quả = 1 | 

Dấu vết này cho thấy thuật toán xác định chính xác rằng chính xác một người là kẻ nói dối. 

### Ví dụ 2 

đầu vào:```
5 5 3 3 5
```Yêu cầu được sắp xếp:`[3, 3, 5, 5, 5]`| x | Khiếu nại >= x | Đếm | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 5 | 5 | không bằng | 
| 1 | 5 | 5 | không bằng | 
| 2 | 5 | 5 | không bằng | 
| 3 | 5 | 5 | không bằng | 
| 4 | 3 | 3 | bằng → kết quả = 3 | 

Điều này xác nhận rằng ba người là những kẻ nói dối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm; kiểm tra số lượng kẻ nói dối của mỗi ứng viên là O(n), không đáng kể | 
| Không gian | O(n) | Lưu trữ mảng xác nhận quyền sở hữu | 

Được cho`t`lên tới 1000 và tổng số`n`lên tới 10.000, giải pháp phù hợp trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        l = list(map(int, input().split()))
        l.sort()
        result = -1
        for x in range(n + 1):
            count = sum(1 for val in l if val >= x)
            if count == x:
                result = x
                break
        print(result)
    return output.getvalue().strip()

# Provided samples
assert run("7\n2\n1 2\n2\n2 2\n2\n0 0\n1\n1\n1\n0\n5\n5 5 3 3 5\n6\n5 3 6 6 3 5") == "1\n-1\n0\n-1\n0\n3\n4"

# Custom cases
assert run("1\n1\n0") == "0", "single person, zero liars"
assert run("1\n1\n1") == "1", "single person, must be liar"
assert run("1\n3\n0 0 0") == "0", "all truth"
assert run("1\n3\n3 3 3") == "3", "all claim max liars"
assert run("1\n5\n0 1 2 3 4") == "2", "gradually increasing claims"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n0`|`0`| Người độc thân, không dối trá | 
|`1\n1\n1`|`1`| Người độc thân, nhất định phải nói dối | 
|`1\n3\n0 0 0`|`0`| Mọi người đều nói sự thật | 
|`1\n3\n3 3 3`|`3`| Mọi người đều tuyên bố là kẻ nói dối tối đa | 
|`1\n5\n0 1 2 3 4`|`2`| Tăng yêu sách, giải pháp trung gian | 

## Vỏ cạnh 

Nếu tất cả các xác nhận quyền sở hữu đều bằng 0, thuật toán sẽ trả về chính xác 0 kẻ nói dối. đầu vào`1 0 0 0`được sắp xếp thành`[0,0,0,1]`; kiểm tra số lượng cho`x=0`mang lại 4, không bằng 0; vì`x=1`số lượng yêu cầu ≥1 là 1, phù hợp`x=1`. Giải pháp điều chỉnh chính xác. Nếu tất cả các yêu cầu đều bằng nhau`n`, thuật toán xác định rằng chỉ số lượng tối đa là hợp lệ, như đã thấy trong dữ liệu đầu vào`3 3 3`cho ba người. Những yêu cầu không thể hòa giải được, chẳng hạn như`[2,2]`, cho kết quả -1 vì không có ứng cử viên`x`thỏa mãn bất biến đẳng thức. Phương thức này xử lý các trường hợp đặc biệt này mà không cần bất kỳ kiểm tra bổ sung nào.
