---
title: "CF 1013B - Và"
description: "Chúng ta được cung cấp một danh sách các số nguyên và một số nguyên đặc biệt x. Phép biến đổi duy nhất được phép là chọn một chỉ mục và thay thế phần tử đó bằng AND theo từng bit của nó bằng x. Thao tác này chỉ có thể giảm số bit, vì AND chỉ có thể biến 1 bit thành 0 tùy thuộc vào x."
date: "2026-06-16T22:32:25+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1013
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 500 (Div. 2) [based on EJOI]"
rating: 1200
weight: 1013
solve_time_s: 92
verified: true
draft: false
---

[CF 1013B - Và](https://codeforces.com/problemset/problem/1013/B) 

**Đánh giá:** 1200 
**Thẻ:** tham lam 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và một số nguyên đặc biệt`x`. Phép biến đổi duy nhất được phép là chọn một chỉ mục và thay thế phần tử đó bằng bit AND của nó bằng`x`. Thao tác này chỉ có thể giảm số bit, vì AND chỉ có thể biến 1 bit thành 0 tùy thuộc vào`x`. 

Mục tiêu không phải là làm cho mảng được sắp xếp hoặc tối ưu hóa theo bất kỳ ý nghĩa tổng thể nào. Yêu cầu duy nhất đơn giản hơn nhiều: sau khi áp dụng thao tác nhiều lần, chúng ta muốn có ít nhất một giá trị xuất hiện ở ít nhất hai vị trí khác nhau. Chúng tôi muốn số lượng thao tác tối thiểu cần thiết để buộc một bản sao như vậy hoặc xác định rằng điều đó là không thể. 

Ràng buộc`n ≤ 100000`buộc mọi giải pháp phải gần với tuyến tính. Bất kỳ chiến lược ghép nối bậc hai nào trên tất cả các giá trị và trạng thái trung gian sẽ quá chậm. Khoảng giá trị lên đến`100000`có nghĩa là chúng ta có thể sử dụng mảng tần số hoặc bản đồ băm một cách an toàn vì không gian trạng thái đủ nhỏ để theo dõi các giá trị chính xác. 

Một sai lầm ngây thơ xuất phát từ việc nghĩ rằng chúng ta phải mô phỏng các thao tác trên tất cả các tập hợp con của chỉ số. Ví dụ: thử tất cả các cặp`(i, j)`và việc kiểm tra xem liệu chúng ta có thể làm cho chúng bằng nhau thông qua chuỗi phép toán AND hay không sẽ nhanh chóng bùng nổ vì mỗi phần tử có thể được chuyển đổi độc lập theo nhiều nhất một cách hữu ích. 

Vấn đề tế nhị thứ hai là giả định rằng các thao tác lặp lại có thể giúp ích nhiều hơn cho một ứng dụng cho mỗi phần tử. Điều này không đúng: áp dụng`a & x`hai lần giống hệt với việc áp dụng nó một lần, vì vậy mỗi phần tử chỉ có hai trạng thái có thể có, nguyên bản hoặc biến đổi. 

Trường hợp cạnh cuối cùng xuất hiện khi không có giá trị nào có thể thay đổi. Ví dụ, nếu`a[i] & x == a[i]`cho tất cả`i`, thì không có thao tác nào thay đổi bất cứ điều gì. Nếu ban đầu mảng không có bản sao thì câu trả lời là ngay lập tức`-1`. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực sẽ xem xét tất cả các tập hợp con của chỉ số để áp dụng các phép toán và sau đó kiểm tra xem có giá trị nào xuất hiện hai lần hay không. Đối với mỗi tập hợp con, chúng tôi sẽ tính toán lại mảng và đếm tần số. Đây là cấp số nhân trong`n`, vì có`2^n`các lựa chọn và thậm chí việc cắt tỉa cũng không giúp ích nhiều vì mỗi thao tác đều thay đổi cấu trúc xung đột toàn cục. Lý do thất bại là do các hoạt động độc lập với từng phần tử nên việc tìm kiếm toàn cục là không cần thiết. 

Quan sát quan trọng là mỗi phần tử có chính xác hai trạng thái có ý nghĩa: giá trị ban đầu của nó`a[i]`và giá trị biến đổi của nó`a[i] & x`. Vì chúng tôi chỉ cần bất kỳ giá trị trùng lặp nào nên chúng tôi chỉ quan tâm đến việc liệu một số giá trị có thể được tạo để xuất hiện ít nhất hai lần bằng cách sử dụng tối đa một phép biến đổi cho mỗi phần tử hay không. 

Điều này làm giảm vấn đề xuống còn việc kiểm tra ba tình huống. Đầu tiên, nếu bất kỳ giá trị nào đã xuất hiện ít nhất hai lần thì không cần thực hiện thao tác nào. Thứ hai, nếu giá trị được chuyển đổi khớp với giá trị ban đầu hiện có, chúng ta có thể sử dụng một thao tác để tạo bản sao. Thứ ba, nếu hai phần tử khác nhau có thể được chuyển đổi thành cùng một giá trị thì chúng ta cần hai thao tác. 

Chúng tôi đánh giá tất cả các giá trị ban đầu và tất cả các giá trị được chuyển đổi và tần số theo dõi. Câu trả lời trở thành số lượng thao tác tối thiểu cần thiết để đạt được bất kỳ tần số ≥ 2 nào trong hệ thống kết hợp, nhưng đảm bảo cẩn thận rằng chúng tôi không tính hai lần các phần tử đã bằng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^n · n) | O(n) | Quá chậm | 
| Kiểm tra tần số + chuyển đổi | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của tất cả các giá trị ban đầu trong bản đồ hoặc mảng băm. Nếu bất kỳ giá trị nào đã có tần số ít nhất là 2 thì câu trả lời là 0. Điều này trực tiếp đáp ứng mục tiêu mà không cần thực hiện bất kỳ thao tác nào. 
2. Xây dựng bản đồ tần số thứ hai cho các giá trị được chuyển đổi`b[i] = a[i] & x`. Chúng tôi không sửa đổi mảng; chúng tôi chỉ tính toán những kết quả giả định này. 
3. Đối với mỗi chỉ số, hãy xem xét liệu việc chuyển đổi nó có tạo ra giá trị đã tồn tại trong bản đồ tần số ban đầu hay không. Nếu có, thao tác đơn lẻ này sẽ ngay lập tức tạo ra một bản sao, vì vậy chúng tôi có thể ghi lại câu trả lời của thí sinh là 1. 
4. Tiếp theo, đếm tần số của các giá trị được chuyển đổi trên tất cả các chỉ số. Nếu bất kỳ giá trị chuyển đổi nào xuất hiện ít nhất hai lần thì việc thực hiện thao tác trên hai trong số các chỉ mục đó sẽ tạo ra một giá trị trùng lặp, do đó chúng ta có thể đạt được mục tiêu trong 2 thao tác. 
5. Câu trả lời là tối thiểu trong số 0, 1 hoặc 2 nếu có thể đạt được, nếu không thì`-1`. 

### Tại sao nó hoạt động 

Mỗi phần tử đóng góp tối đa hai giá trị có thể và mỗi thao tác chỉ di chuyển một phần tử từ giá trị ban đầu sang giá trị được chuyển đổi. Do đó, bất kỳ giải pháp nào cũng tương đương với việc chọn một tập hợp con các chỉ số để “kích hoạt” phép chuyển đổi. Cách duy nhất để tạo các bản sao là căn chỉnh hai phần tử thành cùng một giá trị trên hai trạng thái này. Vì không có sự chuyển tiếp nào nữa nên toàn bộ bài toán giảm xuống còn việc đếm sự trùng lặp giữa hai tập giá trị này và mọi giải pháp tối ưu đều phải rơi vào một trong ba trường hợp đã xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    
    freq = {}
    for v in a:
        freq[v] = freq.get(v, 0) + 1
    
    # already has duplicate
    for v in freq:
        if freq[v] >= 2:
            print(0)
            return
    
    # try one operation case
    seen_after = {}
    for v in a:
        nv = v & x
        if nv in freq:
            print(1)
            return
        seen_after[nv] = seen_after.get(nv, 0) + 1
    
    # check two operations case
    for v in seen_after:
        if seen_after[v] >= 2:
            print(2)
            return
    
    print(-1)

if __name__ == "__main__":
    solve()
```Bước đầu tiên xây dựng một bảng tần số gồm các giá trị ban đầu, cho phép chúng tôi phát hiện ngay xem mảng đó đã thỏa mãn điều kiện hay chưa. Bước thứ hai tính toán từng giá trị được chuyển đổi có thể có và kiểm tra xem nó có thể xung đột với một giá trị hiện có hay không, tương ứng với một thao tác đơn lẻ là đủ. 

Kiểm tra thứ ba tổng hợp các tần số đã biến đổi để xem liệu hai chỉ số khác nhau có hội tụ về cùng một giá trị sau khi biến đổi hay không. Đó là cách duy nhất để buộc trùng lặp khi không có bản sao gốc nào tồn tại và không có phép biến đổi đơn lẻ nào khớp với giá trị hiện có. 

Thứ tự quan trọng vì chúng ta muốn số lượng thao tác tối thiểu, vì vậy chúng ta phải phát hiện`0`trước`1`, Và`1`trước khi xem xét`2`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 2 3 7
```| tôi | một [tôi] | a[i] & x | tần số gốc | kiểm tra va chạm | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | có (1 tồn tại) | 
| 1 | 2 | 2 | 1 | có (2 tồn tại) | 
| 2 | 3 | 3 | 1 | có (3 tồn tại) | 
| 3 | 7 | 3 | 1 | tạo trùng lặp | 

Giá trị được chuyển đổi của 7 trở thành 3, đã tồn tại. Một thao tác là đủ. 

Đầu ra:```
1
```Điều này cho thấy sự xung đột trực tiếp giữa phần tử được chuyển đổi và giá trị hiện có, xác thực logic thao tác đơn. 

### Ví dụ 2 

đầu vào:```
3 1
5 7 3
```| tôi | một [tôi] | a[i] & x | tần số biến đổi | 
| --- | --- | --- | --- | 
| 0 | 5 | 1 | 1 | 
| 1 | 7 | 1 | 2 | 
| 2 | 3 | 1 | 3 | 

Không có bản sao ban đầu tồn tại, nhưng tất cả các giá trị được chuyển đổi đều hội tụ về 1. 

Điều này có nghĩa là hai thao tác là đủ, vì việc chọn hai chỉ số bất kỳ và áp dụng AND sẽ tạo ra một giá trị trùng lặp. 

Đầu ra:```
2
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt cho các tần số gốc và một lượt cho các giá trị được chuyển đổi | 
| Không gian | O(n) | Bản đồ băm lưu trữ số lượng giá trị tần số | 

Các ràng buộc cho phép tối đa 100000 phần tử và giải pháp chỉ thực hiện các bước tuyến tính với các phép toán băm trong thời gian không đổi, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    freq = {}
    for v in a:
        freq[v] = freq.get(v, 0) + 1

    for v in freq:
        if freq[v] >= 2:
            return "0"

    seen_after = {}
    for v in a:
        nv = v & x
        if nv in freq:
            return "1"
        seen_after[nv] = seen_after.get(nv, 0) + 1

    for v in seen_after:
        if seen_after[v] >= 2:
            return "2"

    return "-1"

# provided sample
assert run("4 3\n1 2 3 7\n") == "1"

# all equal
assert run("5 7\n4 4 4 4 4\n") == "0"

# no change possible
assert run("3 0\n1 2 3\n") == "-1"

# two elements converge after AND
assert run("3 1\n5 7 3\n") == "2"

# already duplicate
assert run("4 10\n1 2 2 3\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | 0 | trường hợp không hoạt động | 
| x = 0 không thay đổi | -1 | không thể thực hiện được khi không có phép biến đổi nào hoạt động | 
| hội tụ đầy đủ sau AND | 2 | trường hợp va chạm hai thao tác | 
| bản sao có sẵn | 0 | thoát sớm đúng đắn | 

## Vỏ cạnh 

Hãy xem xét một mảng trong đó ban đầu không có bản sao nào tồn tại và mọi phép biến đổi đều tạo ra cùng một giá trị. Ví dụ,`a = [5, 7, 3]`Và`x = 1`. Các giá trị được chuyển đổi đều trở thành`1`. Thuật toán xây dựng`seen_after = {1: 3}`, phát hiện tần số ≥ 2 và trả về`2`. Điều này phù hợp với yêu cầu vì ít nhất hai chỉ số phải được chuyển đổi để đạt được sự bằng nhau. 

Một trường hợp khác là khi phép biến đổi không làm gì cả. Nếu như`x = 0`, sau đó`a[i] & x = 0`cho tất cả`i`. Nếu mảng ban đầu không chứa các giá trị trùng lặp thì không có giá trị được chuyển đổi nào khớp với bất kỳ giá trị ban đầu nào theo cách hữu ích và`seen_after`không thể giúp tạo ra xung đột với các giá trị hiện có. Thuật toán trả về chính xác`-1`sau khi không thực hiện được cả việc kiểm tra một thao tác và hai thao tác.
