---
title: "CF 105580A - Dĩa"
description: "Chúng ta có năm tập hợp, mỗi tập hợp chứa chính xác năm số nguyên. Mỗi số nguyên mô tả một ngã ba theo số lượng răng mà nó có. Hai nhánh được coi là giống hệt nhau nếu nhãn số nguyên của chúng khớp nhau. Một “tập hợp” ở đây thực sự là một tập hợp gồm năm số nguyên."
date: "2026-06-22T17:48:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105580
codeforces_index: "A"
codeforces_contest_name: "Open Udmurtia High School Programming Contest 2015"
rating: 0
weight: 105580
solve_time_s: 64
verified: true
draft: false
---

[CF 105580A - Dĩa](https://codeforces.com/problemset/problem/105580/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có năm tập hợp, mỗi tập hợp chứa chính xác năm số nguyên. Mỗi số nguyên mô tả một ngã ba theo số lượng răng mà nó có. Hai nhánh được coi là giống hệt nhau nếu nhãn số nguyên của chúng khớp nhau. 

Một “tập hợp” ở đây thực sự là một tập hợp gồm năm số nguyên. Thứ tự bên trong một tập hợp không quan trọng, chỉ là mỗi giá trị xuất hiện bao nhiêu lần. Nhiệm vụ là xác định có bao nhiêu tập hợp riêng biệt xuất hiện trong số năm tập hợp đã cho. 

Vì vậy, đầu vào là năm dòng, mỗi dòng đại diện cho một tập hợp có kích thước năm. Đầu ra là một số nguyên duy nhất: có bao nhiêu tập hợp duy nhất tồn tại trong năm dòng đó. 

Những hạn chế là cực kỳ nhỏ. Chỉ có năm bộ, mỗi bộ có kích thước năm và các giá trị là các số nguyên dương nhỏ lên tới 100. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào chuẩn hóa từng bộ và so sánh chúng sẽ chạy ngay lập tức. Ngay cả việc sắp xếp O(n log n) trên mỗi bộ cũng không đáng kể vì n được cố định ở mức 5. 

Một sai lầm ngây thơ là so sánh các dòng dưới dạng chuỗi thô. Ví dụ, điều trị`[1 2 3 4 5]`Và`[5 4 3 2 1]`khác nhau sẽ không chính xác vì vấn đề bỏ qua thứ tự. Một cạm bẫy phổ biến khác là quên rằng các bản sao bên trong một tập hợp là rất quan trọng, vì vậy`[1 1 2 3 4]`khác với`[1 2 2 3 4]`. 

Các trường hợp cạnh chủ yếu là về hoán vị: 

- Đầu vào:```
1 2 3 4 5
5 4 3 2 1
1 2 3 4 5
1 1 1 1 1
1 1 1 1 1
```Đầu ra đúng là`2`. Một so sánh chuỗi ngây thơ sẽ xuất ra`4`bởi vì nó coi các hoán vị là khác nhau. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là so sánh mọi bộ với mọi bộ khác. Đối với mỗi cặp, chúng tôi kiểm tra xem chúng có đại diện cho cùng một tập hợp hay không bằng cách sắp xếp cả hai và so sánh từng phần tử. Với năm bộ, đây là tối đa 25 phép so sánh và mỗi bộ so sánh sắp xếp năm phần tử, vì vậy điều này hoàn toàn không đáng kể. 

Quan sát quan trọng là chúng ta hoàn toàn không cần so sánh từng cặp. Nếu chúng ta chuyển đổi từng tập hợp thành một biểu diễn chính tắc không phụ thuộc vào thứ tự thì chúng ta có thể chỉ cần chèn các biểu diễn này vào một tập hợp băm và đếm xem chúng ta nhận được bao nhiêu biểu diễn duy nhất. 

Dạng chính tắc tự nhiên là bộ được sắp xếp của năm số nguyên. Việc sắp xếp loại bỏ sự mơ hồ về thứ tự trong khi vẫn bảo toàn được nhiều thông tin. Khi mọi tập hợp được chuyển đổi, tính khác biệt sẽ trở thành phép kiểm tra tính duy nhất đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force So sánh theo cặp | O(25 · 5 log 5) | O(1) | Đã chấp nhận | 
| Canonicalization + Bộ | O(5 log 5) | O(5) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách chuyển đổi từng bộ trong số năm bộ thành dạng chuẩn hóa và đếm xem có bao nhiêu dạng chuẩn hóa khác nhau xuất hiện. 

1. Đọc mỗi dòng trong năm dòng dưới dạng danh sách năm số nguyên. Bước này chỉ tải biểu diễn thô của từng bộ mà không đưa ra các giả định về thứ tự. 
2. Với mỗi danh sách, hãy sắp xếp năm số nguyên theo thứ tự không giảm. Việc sắp xếp được sử dụng vì việc nhận dạng một tập hợp chỉ phụ thuộc vào tần số chứ không phụ thuộc vào thứ tự. Sau khi sắp xếp, các tập hợp giống hệt nhau sẽ luôn tạo ra các chuỗi giống hệt nhau. 
3. Chuyển danh sách đã sắp xếp thành một bộ. Điều này làm cho nó có thể băm được để có thể được lưu trữ trong bộ Python. 
4. Chèn từng bộ dữ liệu vào một tập hợp các cấu hình đã thấy. Bộ này tự động loại bỏ các bản sao giống hệt nhau. 
5. Xuất ra kích thước của bộ, đại diện cho số lượng bộ nĩa riêng biệt. 

### Tại sao nó hoạt động 

Việc sắp xếp từng bộ tạo ra một biểu diễn chuẩn xác định cho cấu trúc nhiều bộ của nó. Hai tập hợp bằng nhau khi và chỉ khi cách biểu diễn được sắp xếp của chúng giống hệt nhau. Vì thuật toán lưu trữ chính xác một biểu diễn cho mỗi tập hợp riêng biệt nên số đếm cuối cùng chính xác là số lượng tập hợp duy nhất trong đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    seen = set()

    for _ in range(5):
        arr = list(map(int, input().split()))
        arr.sort()
        seen.add(tuple(arr))

    print(len(seen))

if __name__ == "__main__":
    main()
```Ý tưởng cốt lõi là chuyển đổi mỗi hàng thành một bộ được sắp xếp. Việc sắp xếp đảm bảo rằng các hoán vị được thu gọn vào cùng một cách biểu diễn. Việc chuyển đổi bộ dữ liệu là bắt buộc vì danh sách Python không thể băm được, trong khi các bộ dữ liệu có thể được chèn vào một tập hợp. 

Một điểm tinh tế là chúng ta không cần bất kỳ mảng tần số hoặc cấu trúc đếm nào vì việc sắp xếp là đủ với kích thước cố định nhỏ của mỗi bộ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 7 1 1
1 7 1 7 1
7 1 1 1 7
7 7 7 7 7
7 1 1 1 7
```Các phép biến đổi được sắp xếp: 

| Đặt | Mẫu sắp xếp | 
| --- | --- | 
| 1 | (1,1,1,1,7) | 
| 2 | (1,1,1,7,7) | 
| 3 | (1,1,1,1,7) | 
| 4 | (7,7,7,7,7) | 
| 5 | (1,1,1,1,7) | 

Sau khi chèn vào một bộ, chúng ta nhận được ba bộ dữ liệu duy nhất. 

Đầu ra:```
3
```Điều này cho thấy rằng nhiều hoán vị sẽ thu gọn một cách chính xác thành các dạng chính tắc giống hệt nhau. 

### Ví dụ 2 

đầu vào:```
1 1 2 2 1
1 2 3 1 1
2 3 1 1 1
1 2 3 1 1
1 1 2 2 1
```Các hình thức sắp xếp: 

| Đặt | Mẫu sắp xếp | 
| --- | --- | 
| 1 | (1,1,1,2,2) | 
| 2 | (1,1,1,2,3) | 
| 3 | (1,1,1,2,3) | 
| 4 | (1,1,1,2,3) | 
| 5 | (1,1,1,2,2) | 

Số lượng duy nhất là 2. 

Đầu ra:```
2
```Ví dụ này xác nhận rằng các cấu trúc lặp lại được hợp nhất chính xác ngay cả khi chúng xuất hiện không liền kề nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(5 log 5) | Sắp xếp năm phần tử mỗi bộ, thực hiện năm lần | 
| Không gian | O(5) | Lưu trữ tối đa năm bộ dữ liệu trong một bộ | 

Các giới hạn có kích thước không đổi, do đó, giải pháp chạy trong thời gian không đổi trong thực tế và thấp hơn nhiều so với bất kỳ ngưỡng hiệu suất nào. Ngay cả với đầu vào lớn hơn nhiều, cách tiếp cận tương tự sẽ thay đổi tuyến tính theo số lượng bộ với chi phí không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    import sys as _sys
    input = _sys.stdin.readline

    seen = set()
    for _ in range(5):
        arr = list(map(int, input().split()))
        arr.sort()
        seen.add(tuple(arr))
    print(len(seen))

    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided sample-style test
assert run("""1 1 7 1 1
1 7 1 7 1
7 1 1 1 7
7 7 7 7 7
7 1 1 1 7
""") == "3"

# all identical
assert run("""1 1 1 1 1
1 1 1 1 1
1 1 1 1 1
1 1 1 1 1
1 1 1 1 1
""") == "1"

# all distinct
assert run("""1 2 3 4 5
1 2 3 4 6
1 2 3 4 7
1 2 3 4 8
1 2 3 4 9
""") == "5"

# permutation duplicates
assert run("""5 4 3 2 1
1 2 3 4 5
5 1 4 2 3
3 2 1 5 4
4 3 2 1 5
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các bộ giống hệt nhau | 1 | chống trùng lặp hoàn toàn | 
| tất cả các bộ riêng biệt | 5 | không có sự hợp nhất ngẫu nhiên | 
| chỉ hoán vị | 1 | trật tự bất biến | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các tập hợp đều là hoán vị của cùng một tập hợp. Ví dụ, bất kỳ sự sắp xếp lại nào của`[1,2,3,4,5]`nên được coi là giống hệt nhau. Sau khi sắp xếp, mỗi hàng sẽ trở thành`(1,2,3,4,5)`và tập hợp sẽ thu gọn chính xác về kích thước một. 

Một trường hợp cạnh khác là khi có các giá trị lặp lại bên trong một tập hợp nhưng được phân bổ khác nhau giữa các tập hợp. Ví dụ,`[1,1,2,2,3]`so với`[1,2,2,3,3]`phải duy trì sự khác biệt. Việc sắp xếp duy trì tính đa bội một cách chính xác, do đó các bộ dữ liệu chính tắc khác nhau và không xảy ra việc hợp nhất không chính xác.
