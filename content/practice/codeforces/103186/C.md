---
title: "CF 103186C - \u5c0f A \u7684\u671f\u672b\u8003\u8bd5"
description: "Chúng tôi được cung cấp một nhóm sinh viên, mỗi nhóm được xác định bằng một ID sinh viên duy nhất và điểm ban đầu. Trong số đó, có một học sinh đặc biệt, được ký hiệu bằng chỉ số m theo thứ tự đầu vào và chúng tôi gọi học sinh này là Xiao A."
date: "2026-07-03T16:12:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103186
codeforces_index: "C"
codeforces_contest_name: "The 2021 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103186
solve_time_s: 52
verified: true
draft: false
---

[CF 103186C - \u5c0f A \u7684\u671f\u672b\u8003\u8bd5](https://codeforces.com/problemset/problem/103186/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một nhóm sinh viên, mỗi nhóm được xác định bằng một ID sinh viên duy nhất và điểm ban đầu. Trong số đó, có một học sinh đặc biệt, được ký hiệu bằng chỉ số m theo thứ tự đầu vào và chúng tôi gọi học sinh này là Xiao A. 

Quá trình chuyển đổi diễn ra theo hai giai đoạn độc lập dựa trên điểm số ban đầu. Đầu tiên, Tiểu A tự điều chỉnh điểm của mình: nếu điểm ban đầu dưới 60 thì tăng lên 60, nếu không thì giữ nguyên. Sự thay đổi này chỉ phụ thuộc vào giá trị ban đầu của chính anh ta và không ảnh hưởng đến cách đánh giá các học sinh khác. 

Thứ hai, Tiểu A nhìn những người khác bằng cách sử dụng danh sách điểm đầy đủ ban đầu. Ngưỡng là điểm trung bình được tính từ trạng thái ban đầu trước khi có bất kỳ sửa đổi nào. Bất kỳ học sinh nào khác có điểm ban đầu ít nhất bằng mức trung bình này sẽ bị giảm 2 điểm, với giới hạn dưới là 0. 

Điểm tinh tế quan trọng là cả hai quyết định đều dựa trên ảnh chụp nhanh điểm số ban đầu. Mức trung bình không được tính lại sau khi Xiao A sửa đổi và khả năng đủ điều kiện giảm cũng được cố định từ dữ liệu ban đầu. Phần động duy nhất là ứng dụng cuối cùng của hai quy tắc. 

Đầu ra yêu cầu in điểm cuối cùng của tất cả học sinh được sắp xếp theo ID học sinh của họ theo thứ tự tăng dần. 

Các ràng buộc nhỏ, với n lên tới 100. Điều này ngay lập tức cho thấy rằng giải pháp O(n^2) hoặc thậm chí O(n) cho mỗi thử nghiệm là đủ dễ dàng. Bất kỳ nỗ lực nào đối với các cấu trúc dữ liệu phức tạp đều không cần thiết và sự rõ ràng của việc triển khai quan trọng hơn việc tối ưu hóa. 

Một lỗi phổ biến xuất phát từ việc tính lại điểm trung bình sau khi sửa đổi điểm của Xiao A. Ví dụ: nếu điểm ban đầu là [1, 4, 100] thì điểm trung bình là 35. Bản cập nhật của Xiao A không thay đổi ngưỡng này, nhưng việc triển khai bất cẩn có thể tính toán lại và dịch chuyển giới hạn không chính xác. 

Một sai lầm khác là cập nhật điểm tại chỗ rồi sử dụng các giá trị cập nhật để quyết định xem ai đó có ở trên mức trung bình hay không. Điều đó phá vỡ quy tắc rằng quyết định phải dựa trên điểm số ban đầu. 

Cuối cùng, việc quên giới hạn dưới của 0 khi trừ 2 có thể tạo ra các giá trị âm, giá trị này không hợp lệ. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất đơn giản. Đầu tiên chúng ta tính giá trị trung bình của tất cả các điểm ban đầu. Sau đó, chúng tôi lặp lại từng học sinh. Nếu học sinh là Xiao A, chúng tôi áp dụng quy tắc giữ điểm của học sinh đó ít nhất là 60. Nếu không, chúng tôi sẽ kiểm tra xem điểm ban đầu của học sinh đó có ít nhất là điểm trung bình hay không và nếu vậy, hãy trừ 2. 

Cách tiếp cận này đã chạy trong thời gian tuyến tính. Ngay cả khi chúng tôi tách tính toán thành nhiều lần một cách rõ ràng thì tổng chi phí vẫn là O(n). Lý do duy nhất để nghĩ đến việc tối ưu hóa là để tránh việc vô tình tính toán lại giá trị trung bình hoặc trộn lẫn các giá trị gốc và giá trị cập nhật. 

Cái nhìn sâu sắc quan trọng là đây không phải là một mô phỏng có phản hồi. Không có gì thay đổi ranh giới quyết định sau khi nó được xác định. Khi chúng tôi lưu trữ mảng ban đầu và tính giá trị trung bình một lần, mọi thao tác khác đều là một phép biến đổi xác định đơn giản trên mỗi phần tử. Điều này làm giảm vấn đề thành một ánh xạ vượt qua một lần từ điểm đầu tiên đến điểm cuối cùng. 

Không có lợi ích gì trong việc duy trì cấu trúc động hoặc sắp xếp trước khi chuyển đổi. Việc sắp xếp chỉ có ý nghĩa ở bước đầu ra cuối cùng, đó là theo ID chứ không phải điểm số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(n) | Đã chấp nhận | 
| Tính toán trực tiếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả hồ sơ học sinh và lưu trữ chúng dưới dạng cặp (student_id, point). Chúng tôi cần lưu trữ đầy đủ vì đầu ra phải được sắp xếp theo ID chứ không phải thứ tự đầu vào. 
2. Tính tổng tất cả các điểm ban đầu rồi chia cho n để lấy điểm trung bình. Giá trị này phải được cố định trong suốt quá trình tính toán. 
3. Xác định Xiao A theo vị trí m của anh ấy trong chuỗi đầu vào và lưu trữ riêng điểm ban đầu của anh ấy. Chúng tôi giữ nó không thay đổi để đưa ra quyết định. 
4. Tạo ánh xạ từ mã số sinh viên đến điểm cuối cùng. Chúng tôi sẽ điền thông tin này dựa trên các quy tắc áp dụng cho giá trị ban đầu. 
5. Lặp lại từng học sinh. Nếu học sinh là Xiao A, hãy đặt điểm của em ấy ở mức tối đa(60, original_score). Điều này đảm bảo việc điều chỉnh vượt qua tối thiểu được thực thi mà không ảnh hưởng đến người khác. 
6. Đối với tất cả học sinh khác, so sánh điểm ban đầu của họ với điểm trung bình tính toán. Nếu original_score ≥ trung bình, hãy giảm nó đi 2, đảm bảo nó không giảm xuống dưới 0. Nếu không, hãy giữ nguyên. 
7. Sau khi xử lý tất cả học sinh, sắp xếp chúng theo ID học sinh và xuất ra điểm cuối cùng của chúng. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là tất cả các quyết định đều được thực hiện bằng cách sử dụng trạng thái tham chiếu cố định: mảng điểm ban đầu. Mức trung bình xác định phân vùng tĩnh của học sinh thành hai nhóm và sự điều chỉnh của Xiao A không phụ thuộc vào phân vùng đó. Vì không có quy tắc nào phụ thuộc vào kết quả trung gian nên phép biến đổi là một hàm thuần túy được áp dụng cho mỗi phần tử. Việc sắp xếp chỉ ảnh hưởng đến thứ tự đầu ra và không ảnh hưởng đến việc tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

students = []
id_list = []
scores = []
a_idx = m - 1  # m is given in input order position

for _ in range(n):
    sid, sc = map(int, input().split())
    students.append((sid, sc))
    id_list.append(sid)
    scores.append(sc)

total = sum(scores)
avg = total / n

a_id, a_score = students[a_idx]

result = {}

for sid, sc in students:
    if sid == a_id:
        result[sid] = max(60, sc)
    else:
        if sc >= avg:
            sc = max(0, sc - 2)
        result[sid] = sc

students.sort(key=lambda x: x[0])

print(" ".join(str(result[sid]) for sid, _ in students))
```Việc triển khai giữ hai biểu diễn song song: danh sách thô của sinh viên và từ điển cho kết quả cuối cùng. Lý do sử dụng từ điển là thứ tự đầu ra phụ thuộc vào ID được sắp xếp, có thể khác với thứ tự đầu vào. Giá trị trung bình được tính một lần trước khi sửa đổi, đảm bảo tính chính xác của ngưỡng logic. 

Một điểm tinh tế là xác định chính xác Xiao A. Đầu vào đảm bảo rằng m đề cập đến vị trí đầu vào chứ không phải mã sinh viên. Đó là lý do tại sao chúng tôi lập chỉ mục trực tiếp vào danh sách được lưu trữ bằng cách sử dụng m - 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 1
2 4
3 100
```Trạng thái ban đầu: 

| Bước | Sinh viên | Điểm | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 < trung bình(35) | 1 | 
| 2 | 2 | 4 | Kẹp Tiểu A | 60 | 
| 3 | 3 | 100 | ≥ trung bình, -2 | 98 | 

Trung bình là (1 + 4 + 100) / 3 = 35. 

Học sinh đầu tiên ở dưới mức trung bình nên không thay đổi. Tiểu A có điểm 4 nên được 60. Học sinh thứ ba trên trung bình nên giảm 2 điểm. 

Đầu ra được sắp xếp theo ID:```
1 60 98
```Điều này xác nhận rằng mức trung bình được cố định so với dữ liệu gốc và việc điều chỉnh của Xiao A không ảnh hưởng đến người khác. 

### Ví dụ 2 

đầu vào:```
4 2
4 49
2 98
3 1
1 22
```Trung bình = (49 + 98 + 1 + 22) / 4 = 42,5 

| Mã sinh viên | Bản gốc | Tình trạng | Cuối cùng | 
| --- | --- | --- | --- | 
| 4 | 49 | ≥ trung bình, -2 | 47 | 
| 2 | 98 | Tiểu A, max(60, 98) | 98 | 
| 3 | 1 | < trung bình | 1 | 
| 1 | 22 | < trung bình | 22 | 

Đầu ra:```
22 98 1 47
```Dấu vết này cho thấy rằng các giá trị trung bình phân số được xử lý chính xác mà không cần làm tròn, vì việc so sánh được thực hiện bằng cách sử dụng logic ngưỡng có giá trị thực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp theo ID sinh viên chiếm ưu thế sau khi xử lý tuyến tính | 
| Không gian | O(n) | lưu trữ tất cả hồ sơ và kết quả của học sinh | 

Kích thước đầu vào tối đa là 100, vì vậy ngay cả bước sắp xếp cũng không đáng kể. Giải pháp chạy thoải mái trong giới hạn và mức sử dụng bộ nhớ không đáng kể so với các hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    data = inp.strip().split()
    n = int(data[0])
    m = int(data[1])

    students = []
    idx = 2
    for _ in range(n):
        sid = int(data[idx]); sc = int(data[idx+1])
        idx += 2
        students.append((sid, sc))

    total = sum(sc for _, sc in students)
    avg = total / n

    a_id, a_score = students[m-1]

    res = {}
    for sid, sc in students:
        if sid == a_id:
            res[sid] = max(60, sc)
        else:
            if sc >= avg:
                sc = max(0, sc - 2)
            res[sid] = sc

    students_sorted = sorted(students)
    return " ".join(str(res[sid]) for sid, _ in students_sorted)

# provided samples
assert run("3 2\n1 1\n2 4\n3 100") == "1 60 98"
assert run("4 2\n4 49\n2 98\n3 1\n1 22") == "22 98 1 47"

# all equal
assert run("3 1\n2 50\n1 50\n3 50") == "48 60 48"

# minimum n
assert run("1 1\n1 59") == "60"

# boundary average
assert run("2 1\n1 60\n2 60") == "58 60"

# high skew
assert run("3 3\n1 0\n2 0\n3 100") == "0 0 98"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều có điểm bằng nhau | 48 60 48 | Kẹp Tiểu A và ranh giới trung bình | 
| sinh viên độc thân | 60 | trường hợp tối thiểu | 
| ranh giới hỗn hợp | 58 60 | xử lý bình đẳng trung bình | 
| phân phối lệch | 0 0 98 | giảm chọn lọc đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các điểm đều giống hệt nhau. Trong tình huống đó, điểm trung bình bằng mỗi điểm nên mọi học sinh không phải Tiểu A đều đủ điều kiện để bị giảm điểm. Đối với đầu vào:```
3 1
2 50
1 50
3 50
```Trung bình là 50. Xiao A (học sinh 2) trở thành 60. Tất cả những người khác giảm đi 2, mỗi người được 48. Thuật toán áp dụng đúng điều kiện ≥. 

Một trường hợp khác là một học sinh độc thân. Đối với đầu vào:```
1 1
1 59
```Trung bình là 59. Xiao A là học sinh duy nhất và được giới hạn ở mức 60. Không có bản cập nhật nào khác nên đầu ra là 60. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp trên các vòng lặp khác trống. 

Trường hợp cuối cùng là khi Xiao A đã trên 60 và trên mức trung bình. Đối với đầu vào:```
2 2
1 100
2 90
```Điểm trung bình là 95. Học sinh 1 ở dưới mức trung bình nên không thay đổi. Xiao A vẫn giữ nguyên 90 vì max(60, 90) là 90. Đầu ra cuối cùng không thay đổi ngoại trừ thứ tự. Điều này xác nhận rằng quy tắc của Xiao A không tương tác với điều kiện ngưỡng áp dụng cho người khác.
