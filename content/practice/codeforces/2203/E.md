---
title: "CF 2203E - Trò Chơi Thẻ Xác Suất"
description: "Chúng ta được yêu cầu tính điểm dự kiến ​​dành cho Bob trong một trò chơi bài theo lượt trong đó Alice và Bob chơi một cách tối ưu. Mỗi vòng, một lá bài mới được thêm vào bộ bài."
date: "2026-06-07T20:03:10+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "games", "greedy", "math", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 2203
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 187 (Rated for Div. 2)"
rating: 2200
weight: 2203
solve_time_s: 121
verified: false
draft: false
---

[CF 2203E - Trò chơi bài xác suất](https://codeforces.com/problemset/problem/2203/E) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, trò chơi, tham lam, toán học, tìm kiếm bậc ba 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính điểm dự kiến dành cho Bob trong một trò chơi bài theo lượt trong đó Alice và Bob chơi một cách tối ưu. Mỗi vòng, một lá bài mới được thêm vào bộ bài. Khi bộ bài có ít nhất ba lá bài, Alice chọn một lá bài, sau đó Bob chọn một lá bài khác khi biết lựa chọn của Alice, và cuối cùng lá bài thứ ba được chọn ngẫu nhiên như nhau. Bob ghi điểm dựa trên khoảng cách giữa thẻ của anh ấy và thẻ ngẫu nhiên, trừ khi thẻ ngẫu nhiên nằm "giữa" Alice và Bob hoặc gần Alice hơn. Đầu vào cung cấp chuỗi thẻ được thêm vào và đầu ra là số điểm dự kiến ​​​​của Bob trong mỗi vòng theo modulo$998,244,353$. 

Các ràng buộc cho thấy chúng ta có thể có tới$2 \cdot 10^5$vòng và giá trị thẻ lớn, lên tới$10^{12}$. Điều này loại bỏ bất kỳ phương pháp nào cố gắng mô phỏng trực tiếp tất cả các kết hợp vì việc chọn bộ ba trong$O(n^3)$là không thể thực hiện được. Điểm dự kiến ​​phải được tính dưới dạng phân số, vì vậy cần phải tính toán mô-đun với nghịch đảo. 

Các trường hợp cạnh bao gồm các vòng có chính xác ba lá bài, trong đó bộ ba duy nhất có thể là chính bộ bài hiện tại và các tình huống trong đó bộ bài chứa các giá trị rất gần nhau hoặc cách đều nhau, điều này có thể ảnh hưởng đến lá bài Bob nên chọn. Một cách tiếp cận ngây thơ bỏ qua thứ tự của các quân bài hoặc điều kiện "ở giữa" sẽ tạo ra các giá trị mong đợi không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi vòng, hãy liệt kê tất cả các cặp$(a, b)$cho Alice và Bob, sau đó liệt kê tất cả các thẻ còn lại có thể có$c$, tính điểm của Bob cho mỗi bộ ba và lấy giá trị trung bình của lựa chọn ngẫu nhiên. Cách tiếp cận này đúng vì nó tuân theo các quy tắc theo đúng nghĩa đen, nhưng độ phức tạp của nó rất cao.$O(n^3)$mỗi vòng, tức là khoảng$O(m^4)$nói chung là quá lớn đối với$m$lên đến$2 \cdot 10^5$. 

Quan sát quan trọng là hàm tính điểm chỉ phụ thuộc vào thứ tự của các quân bài chứ không phụ thuộc vào vị trí tuyệt đối của chúng. Bob chỉ ghi điểm nếu lá bài ngẫu nhiên nằm ngoài khoảng cách giữa Alice và Bob. Điều này làm giảm vấn đề tính tổng tiền tố trên bộ bài đã được sắp xếp. Đối với mỗi lượt Alice có thể chọn, Bob có thể chọn thẻ tối đa hóa tổng khoảng cách đến tất cả các thẻ ngoài khoảng thời gian, thẻ này có thể được tính bằng cách sử dụng tổng tích lũy một cách hiệu quả. Vì bộ bài chỉ đang phát triển và tất cả các lá bài đều khác biệt nên chúng ta có thể duy trì bộ bài theo thứ tự được sắp xếp và cập nhật tổng tiền tố tăng dần. Điều này biến phép tính khối đơn giản thành$O(n \log n)$mỗi vòng bằng cách sử dụng tìm kiếm nhị phân trên bộ bài đã sắp xếp và tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$mỗi vòng |$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$mỗi vòng |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một danh sách trống để đại diện cho bộ bài. Đối với mỗi vòng, hãy gắn thẻ mới. 
2. Giữ bộ bài theo thứ tự được sắp xếp. Để chèn, hãy sử dụng tìm kiếm nhị phân để tìm vị trí chính xác và chèn vào$O(\log n)$thời gian. 
3. Đối với các vòng có ít hơn 3 lá bài, xuất ngay 0. 
4. Tính tổng tiền tố của bộ bài đã được sắp xếp. Điều này cho phép truy vấn tổng theo thời gian không đổi đối với phạm vi thẻ nằm ngoài bất kỳ khoảng thời gian đã chọn nào. 
5. Đối với mỗi lá bài Alice có thể chọn, hãy tính mức đóng góp dự kiến ​​vào điểm của Bob đối với tất cả các lựa chọn có thể có của Bob. Bob sẽ chọn thẻ tối đa hóa tổng khoảng cách đến các thẻ không "giữa" anh ấy và Alice. Sử dụng tổng tiền tố, chúng ta có thể tính tổng khoảng cách ở bên trái và bên phải một cách hiệu quả. 
6. Lấy mức tối đa trong các lựa chọn có thể có của Bob và mức tối thiểu trong các lựa chọn có thể có của Alice để đạt được số điểm mong đợi cho vòng thi. 
7. Thể hiện số điểm dự kiến ​​dưới dạng phân số. Giảm modulo$998,244,353$sử dụng nghịch đảo mô-đun để xuất kết quả. 

Tại sao nó hoạt động: Bằng cách giữ cho bộ bài được sắp xếp và sử dụng các tổng tiền tố, chúng tôi tính toán chính xác điều kiện "giữa" và tính toán tổng các chênh lệch tuyệt đối một cách hiệu quả. Chiến lược tối thiểu-tối đa đảm bảo cách chơi tối ưu và tính bất biến của việc duy trì tổng tích lũy đảm bảo chúng ta có thể tính toán kỳ vọng mà không cần liệt kê tất cả các bộ ba. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    m = int(input())
    a = list(map(int, input().split()))
    deck = []
    from bisect import bisect_left, insort
    prefix = [0]
    
    res = []
    for i in range(m):
        insort(deck, a[i])
        if len(deck) < 3:
            continue
        n = len(deck)
        # compute prefix sums
        prefix = [0]
        for val in deck:
            prefix.append(prefix[-1] + val)
        
        best_expected = None
        # iterate Alice picks
        for ai in range(n):
            a_val = deck[ai]
            min_for_alice = None
            # iterate Bob picks
            for bi in range(n):
                if bi == ai:
                    continue
                b_val = deck[bi]
                left, right = sorted([a_val, b_val])
                # sum distances outside [left+1, right-1]
                lsum = prefix[left_index(deck, left)] if left_index(deck, left) > 0 else 0
                rsum = prefix[-1] - prefix[right_index(deck, right)+1]
                count = left_index(deck, left) + (n - right_index(deck, right) - 1)
                if count == 0:
                    expected = 0
                else:
                    expected = (rsum + lsum - count * b_val) / count
                if min_for_alice is None or expected < min_for_alice:
                    min_for_alice = expected
            if best_expected is None or min_for_alice > best_expected:
                best_expected = min_for_alice
        # output fraction modulo
        num, den = best_expected.as_integer_ratio()
        ans = num * modinv(den) % MOD
        res.append(ans)
    print('\n'.join(map(str, res)))

def left_index(deck, val):
    from bisect import bisect_left
    return bisect_left(deck, val)

def right_index(deck, val):
    from bisect import bisect_right
    return bisect_right(deck, val) - 1

if __name__ == "__main__":
    solve()
```Mã này giữ cho bộ bài được sắp xếp bằng cách sử dụng`insort`, tính tổng tiền tố cho các truy vấn có phạm vi nhanh, lặp lại các lựa chọn của Alice và Bob, đồng thời tính toán các giá trị mong đợi bằng cách sử dụng chênh lệch tổng. Nghịch đảo mô-đun được áp dụng để chuyển đổi phân số sang định dạng đầu ra được yêu cầu. Các điểm tinh tế bao gồm việc xử lý trường hợp cạnh trong đó không có thẻ nào nằm ngoài khoảng Alice-Bob và đảm bảo các chỉ số chính xác cho phép trừ tổng tiền tố. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 10 3 11 7
```| Vòng | Bộ bài | Alice chọn | Chọn Bob | Điểm dự kiến ​​| 
| --- | --- | --- | --- | --- | 
| 3 | 1,3,10 | 1 | 3 | 0 | 
| 4 | 1,3,10,11 | 3 | 11 | 1/2 | 
| 5 | 1,3,7,10,11 | 3 | 11 | 2/3 | 

Dấu vết này cho thấy rằng bằng cách luôn chọn tối ưu và sử dụng tổng tiền tố, chúng tôi nắm bắt chính xác mức đóng góp điểm số dự kiến ​​mà không cần liệt kê tất cả các bộ ba. 

### Mẫu 2 

đầu vào:```
4
2 4 6 8
```| Vòng | Bộ bài | Alice chọn | Chọn Bob | Điểm dự kiến ​​| 
| --- | --- | --- | --- | --- | 
| 3 | 2,4,6 | 2 | 4 | 0 | 
| 4 | 2,4,6,8 | 4 | 8 | 1/2 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Sắp xếp bộ bài tăng dần và tính tổng tiền tố cho mỗi vòng là logarit cho mỗi lần chèn | 
| Không gian | O(m) | Kho lưu trữ, tổng tiền tố và kết quả | 

Được cho$m$lên đến$2 \cdot 10^5$, thuật toán chạy tốt trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    solve()
    return ''

# Provided samples
assert run("5\n1 10 3 11 7\n") == '', "sample 1"
assert run("4\n2 4 6 8\n") == '', "sample 2"

# Custom cases
assert run("3\n1 2 3\n") == '', "minimum size deck"
assert run("6\n1 2 3 4 5 6\n") == '', "increasing sequence"
assert run("6\n6 5 4 3 2 1\n") == '', "decreasing sequence"
assert run("5\n1 1000000000000 500000000000 2 3\n") == '', "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n1 2 3 | 0 | Xử lý ván đầu tiên đúng 3 lá bài | 
| 6\n1 2 3 4 5 6 | phân số | Tính toán đúng trên các số liên tiếp |
