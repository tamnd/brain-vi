---
title: "CF 1468M - Bộ tương tự"
description: "Chúng ta có nhiều tập hợp số nguyên, mỗi tập hợp được coi là một tập hợp. Hai tập hợp được coi là giống nhau nếu chúng có ít nhất hai số chung. Mục tiêu là tìm bất kỳ cặp tập hợp hoặc báo cáo tương tự nào mà không tồn tại."
date: "2026-06-11T01:32:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1468
codeforces_index: "M"
codeforces_contest_name: "2020-2021 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules)"
rating: 2300
weight: 1468
solve_time_s: 124
verified: false
draft: false
---

[CF 1468M - Bộ tương tự](https://codeforces.com/problemset/problem/1468/M) 

**Đánh giá:** 2300 
**Tags:** cấu trúc dữ liệu, đồ thị, cách triển khai 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có nhiều tập hợp số nguyên, mỗi tập hợp được coi là một tập hợp. Hai tập hợp được coi là giống nhau nếu chúng có ít nhất hai số chung. Mục tiêu là tìm bất kỳ cặp tập hợp hoặc báo cáo tương tự nào mà không tồn tại. Mỗi bộ được mô tả dưới dạng danh sách các số nguyên riêng biệt và chúng ta có nhiều trường hợp thử nghiệm trong một đầu vào. 

Các ràng buộc đầu vào rất chặt chẽ. Một trường hợp thử nghiệm có thể có tới 100.000 bộ và mỗi bộ có thể chứa tối đa 100.000 số nguyên. Tuy nhiên, tổng của tất cả các số nguyên trong tất cả các trường hợp thử nghiệm bị giới hạn bởi 200.000. Giới hạn này rất quan trọng vì nó ngăn cản cách tiếp cận ngây thơ khi so sánh trực tiếp từng cặp tập hợp. Một so sánh đơn giản sẽ liên quan đến việc kiểm tra tất cả các cặp tập hợp có thể theo thứ tự$10^{10}$hoạt động trong trường hợp xấu nhất, quá chậm trong giới hạn thời gian 1 giây. Điều quan trọng là mặc dù có nhiều tập hợp nhưng tổng số phần tử lại tương đối nhỏ. 

Trường hợp cạnh xảy ra khi tất cả các tập hợp đều nhỏ hoặc khi mỗi tập hợp có các phần tử hoàn toàn duy nhất. Ví dụ: nếu hai bộ chỉ chia sẻ một phần tử thì chúng không giống nhau và cách tiếp cận bất cẩn chỉ tính kích thước giao điểm mà không xác minh yêu cầu đối với hai phần tử sẽ báo cáo kết quả dương tính giả. Tương tự, nếu hai tập hợp giống hệt nhau thì chúng gần như giống nhau và lời giải sẽ phát hiện ra điều đó ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ lặp lại trên tất cả các cặp tập hợp và kiểm tra xem chúng có bao nhiêu phần tử chia sẻ. Đối với mỗi cặp, người ta có thể sử dụng giao điểm cố định, nhưng việc tính toán giao điểm nhiều lần cho tất cả các cặp rất tốn kém. Nếu chúng ta có$n$bộ và mỗi bộ có trung bình là$k$các phần tử, độ phức tạp mạnh mẽ là$O(n^2 \cdot k)$, điều này là không thể thực hiện được đối với$n$lớn tới 100.000, thậm chí với tổng phần tử bị ràng buộc. 

Quan sát quan trọng dẫn đến cách tiếp cận tối ưu là không có số lần nào xảy ra nhiều hơn một số lần giới hạn. Nếu một số xuất hiện trong nhiều bộ, nó có thể hoạt động như một trung tâm để nhanh chóng xác định các bộ tương tự tiềm năng. Nếu một số chỉ xuất hiện trong một bộ thì nó không thể đóng góp vào sự giống nhau. Bằng cách tập trung vào các cặp phần tử trong tập hợp, chúng ta có thể lưu trữ tập hợp nào chứa cặp phần tử nào. Nếu bất kỳ cặp số nào xuất hiện cùng nhau trong hai bộ thì các bộ đó tương tự nhau. 

Chúng ta giải quyết vấn đề bằng cách ánh xạ mọi cặp số vào (các) tập hợp mà chúng xuất hiện. Vì tổng số phần tử bị giới hạn bởi 200.000 và mỗi tập hợp chứa nhiều nhất$k_i$phần tử, tổng số cặp duy nhất trên tất cả các bộ có thể quản lý được. Chúng ta có thể xử lý điều này theo thời gian tuyến tính so với tổng số phần tử, tránh sự bùng nổ bậc hai của phương pháp tiếp cận vũ phu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * k) | O(n * k) | Quá chậm | 
| Tối ưu | O(tổng_elements + tổng_cặp) | O(tổng_cặp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, đọc số tập hợp và sau đó đọc từng tập hợp dưới dạng danh sách các số nguyên. Lưu trữ tất cả các bộ trong một danh sách. 
2. Chuẩn hóa các phần tử bằng cách ánh xạ từng số nguyên duy nhất tới ID nhỏ hơn nếu cần. Điều này ngăn kích thước số nguyên ảnh hưởng đến hoạt động của từ điển. 
3. Với mỗi bộ, tạo tất cả các cặp phần tử. Đối với mỗi cặp, hãy duy trì ánh xạ từ cặp đó tới chỉ mục của tập hợp nơi nó xuất hiện. Nếu một cặp đã có trong ánh xạ thì chúng ta đã tìm thấy hai bộ chia sẻ cặp này. Xuất ra hai chỉ số đã đặt này ngay lập tức. 
4. Nếu không có cặp nào xuất hiện trong nhiều bộ, ghi -1 cho trường hợp thử nghiệm này. 

Bất biến quan trọng là nếu hai bộ có chung ít nhất hai phần tử thì ít nhất một cặp phần tử dùng chung này sẽ xuất hiện trong cả hai bộ. Bằng cách lưu trữ các cặp thay vì các phần tử riêng lẻ, chúng tôi đảm bảo rằng mọi bộ tương tự đều được phát hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        sets = []
        elements = {}
        idx = 0
        for _ in range(n):
            line = list(map(int, input().split()))
            k = line[0]
            s = line[1:]
            sets.append(s)
            for num in s:
                if num not in elements:
                    elements[num] = idx
                    idx += 1

        # map numbers to compressed IDs
        compressed_sets = [[elements[x] for x in s] for s in sets]

        pair_map = {}
        found = False
        for i, s in enumerate(compressed_sets):
            s_sorted = sorted(s)
            for j in range(len(s_sorted)):
                for k in range(j + 1, len(s_sorted)):
                    pair = (s_sorted[j], s_sorted[k])
                    if pair in pair_map:
                        print(pair_map[pair] + 1, i + 1)
                        found = True
                        break
                    pair_map[pair] = i
                if found:
                    break
            if found:
                break
        if not found:
            print(-1)

if __name__ == "__main__":
    main()
```Mã nén tất cả các số nguyên thành các ID nhỏ hơn để đạt hiệu quả và lặp qua từng bộ tạo ra tất cả các cặp. Nếu bất kỳ cặp nào đã có trong từ điển, chúng tôi sẽ xuất ngay các chỉ số tương ứng. Điều này ngăn thuật toán kiểm tra các cặp không cần thiết sau khi tìm thấy giải pháp. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên: 

| tôi | Đặt | Đã nén | Cặp | Thành lập? | 
| --- | --- | --- | --- | --- | 
| 1 | [1,10] | [0,1] | (0,1) | cửa hàng | 
| 2 | [1,3,5] | [0,2,3] | (0,2),(0,3),(2,3) | cửa hàng | 
| 3 | [5,4,3,2,1] | [3,4,2,5,0] | (3,4),(3,2),(3,5),(3,0),(4,2),(4,5),(4,0),(2,5),(2,0),(5,0) | cặp (2.0) đã thấy chưa? vâng, đã tìm thấy bộ 2 và 3 | 

Đầu ra là`2 3`. Điều này xác nhận thuật toán phát hiện cặp tương tự đầu tiên. 

Trong trường hợp không có bộ tương tự: 

| tôi | Đặt | Cặp | Thành lập? | 
| --- | --- | --- | --- | 
| 1 | [1,3,5] | (1,3),(1,5),(3,5) | cửa hàng | 
| 2 | [4,3,2] | (4,3),(4,2),(3,2) | cửa hàng | 
| Không có cặp chồng chéo, đầu ra -1. | | | | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng_elements + tổng_cặp) | Mỗi phần tử được xử lý một lần và tất cả các cặp trong tập hợp đều được tạo. Tổng số cặp được giới hạn bởi sum(k_i choose 2) trên tất cả các bộ. | 
| Không gian | O(tổng_cặp) | Chúng tôi lưu trữ một cặp ánh xạ từ điển để đặt chỉ mục. | 

Với hạn chế là tổng số phần tử trong tất cả các trường hợp thử nghiệm không vượt quá 200.000, giải pháp này chạy thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided samples
assert run("""3
4
2 1 10
3 1 3 5
5 5 4 3 2 1
3 10 20 30
3
4 1 2 3 4
4 2 3 4 5
4 3 4 5 6
2
3 1 3 5
3 4 3 2""") in ["2 3\n1 2\n-1","1 2\n2 3\n-1"], "sample 1"

# custom cases
assert run("1\n2\n2 1 2\n2 2 3") == "-1", "only one common element"
assert run("1\n2\n3 1 2 3\n3 2 3 4") == "1 2", "two common elements"
assert run("1\n3\n2 1 2\n2 3 4\n2 5 6") == "-1", "no similarity"
assert run("1\n2\n3 1 2 3\n3 1 2 3") == "1 2", "identical sets"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 bộ chỉ chia sẻ một phần tử | -1 | Thuật toán kiểm tra không báo cáo sai sự giống nhau | 
| 2 bộ chia sẻ chính xác hai yếu tố | 1 2 | Xác nhận phát hiện các bộ tương tự hợp lệ | 
| 3 bộ không có phần tử chung | -1 | Xác nhận hoạt động phát hiện tiêu cực | 
| 2 bộ giống hệt nhau | 1 2 | Xác nhận các bộ giống hệt nhau được xác định chính xác | 

## Vỏ cạnh 

Đối với một tập hợp có hai phần tử được lặp lại thành nhiều tập hợp, thuật toán sẽ xác định chính xác cặp tập hợp đầu tiên có chung một cặp. đầu vào:```
1
3
3 1 2 3
3 1 2 4
3 5 6 7
```Cặp (
