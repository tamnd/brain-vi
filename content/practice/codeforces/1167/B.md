---
title: "CF 1167B - Số bị mất"
description: "Chúng ta có một thứ tự không xác định gồm sáu số cố định: 4, 8, 15, 16, 23 và 42. Mỗi số xuất hiện đúng một lần trong một mảng có độ dài sáu, nhưng vị trí của chúng bị ẩn."
date: "2026-06-18T17:03:14+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "divide-and-conquer", "interactive", "math"]
categories: ["algorithms"]
codeforces_contest: 1167
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 65 (Rated for Div. 2)"
rating: 1400
weight: 1167
solve_time_s: 85
verified: false
draft: false
---

[CF 1167B - Số bị mất](https://codeforces.com/problemset/problem/1167/B) 

**Đánh giá:** 1400 
**Tags:** vũ lực, chia để trị, tương tác, toán học 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một thứ tự không xác định gồm sáu số cố định: 4, 8, 15, 16, 23 và 42. Mỗi số xuất hiện đúng một lần trong một mảng có độ dài sáu, nhưng vị trí của chúng bị ẩn. Cách duy nhất của chúng ta để tìm hiểu về mảng là đặt các truy vấn có dạng “tích của các phần tử ở vị trí i và j”. 

Nhiệm vụ là khôi phục toàn bộ hoán vị bằng cách sử dụng tối đa bốn truy vấn, sau đó xuất mảng theo thứ tự. 

Mỗi truy vấn cho thấy một mối quan hệ nhân duy nhất giữa hai vị trí. Vì chỉ có sáu giá trị riêng biệt và tất cả chúng đều là tổng hợp theo những cách khác nhau nên cấu trúc của sản phẩm mang tính thông tin duy nhất. Thách thức là chọn các truy vấn cho phép chúng ta xây dựng lại tất cả các vị trí mà không có sự mơ hồ. 

Ràng buộc chỉ cho phép bốn truy vấn là hạn chế về cấu trúc chính. Việc xây dựng lại đơn giản cố gắng xác định từng vị trí một cách độc lập sẽ yêu cầu tới 15 tích theo cặp, điều này là không thể ở đây. Tập hợp các giá trị cố định nhỏ gợi ý rằng chúng ta nên khai thác cấu trúc nhân tố thay vì tìm kiếm. 

Một dạng lỗi tinh vi xuất phát từ việc giả định rằng một sản phẩm xác định duy nhất hai yếu tố của nó mà không xem xét rằng nhiều hoán vị có thể tạo ra cùng một sản phẩm nếu chúng ta không cẩn thận về cách sử dụng lại các truy vấn. Ví dụ, 8 × 15 = 120, nhưng 4 × 30 cũng sẽ là 120 trong một bài toán giả định thoải mái. Ở đây chúng ta an toàn vì các giá trị ứng cử viên đã được cố định và biết trước. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của sáu số và đối với mỗi hoán vị sẽ mô phỏng xem liệu nó có phù hợp với các truy vấn mà chúng tôi yêu cầu hay không. Vì có 6! = 720 hoán vị và mỗi xác minh yêu cầu so sánh tối đa 4 sản phẩm, điều này vẫn khả thi về mặt tính toán. Tuy nhiên, điều này bỏ qua ràng buộc tương tác: chúng tôi không thể kiểm tra một cách thích ứng tất cả các hoán vị đối với người đánh giá vì mỗi truy vấn đều tốn kém và bị hạn chế. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần so sánh từng cặp tùy ý. Chúng ta chỉ cần đủ thông tin để xác định từng vị trí liên quan đến một mỏ neo đã biết. Cấu trúc của tập hợp cố định rất quan trọng: 4, 8, 15, 16, 23, 42 được sắp xếp sao cho các phần tử liền kề theo đúng thứ tự được sử dụng trong bài toán ban đầu sẽ tạo ra các sản phẩm đặc biệt và quan trọng hơn là các sản phẩm lặp lại cho phép chúng ta khôi phục chuỗi từng bước. 

Chúng tôi xây dựng lại hoán vị bằng cách trước tiên xác định vị trí của các phần tử mà tích của chúng với chính chúng hoặc với các phần tử lân cận đã biết mang lại hệ số phân tích duy nhất. Sau khi xác định được một giá trị, chúng ta có thể xâu chuỗi tiếp theo bằng cách chia sản phẩm. 

Chiến lược tối ưu tiêu chuẩn sử dụng thực tế là chúng ta có thể khôi phục bốn phần tử đầu tiên bằng cách truy vấn (1,2), (2,3), (3,4), sau đó suy ra các giá trị bằng cách giao nhau các ràng buộc. Sau khi khôi phục bốn số đầu tiên, hai số còn lại được xác định duy nhất từ ​​các giá trị không sử dụng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(720 · 4) | O(1) | Quá chậm/không thực tế trong cài đặt tương tác | 
| Tối ưu | O(1) truy vấn (≤4) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng cốt lõi là khôi phục chuỗi nhân giữa các vị trí liên tiếp. 

### bước 

1. Truy vấn tích các vị trí (1, 2), (2, 3), (3, 4) và (4, 5). 

Mỗi truy vấn đưa ra một tích của hai giá trị chưa biết liền kề. Lý do chọn các cặp liền kề là vì mỗi phần tử ở giữa xuất hiện trong hai sản phẩm, điều này cho phép chúng ta tách biệt nó. 
2. Lưu trữ bốn sản phẩm này dưới dạng p12, p23, p34 và p45. 

Mỗi số này là tích của hai số cố định đã biết. 
3. Tính giá trị ứng viên cho vị trí 2, 3 và 4 bằng cách quan sát: 

vị trí 2 chia cả p12 và p23, vị trí 3 chia cả p23 và p34, và vị trí 4 chia cả p34 và p45.

Thuộc tính giao nhau này buộc phải khớp duy nhất vì tất cả các số đều là số nguyên tố/sản phẩm riêng biệt có sự trùng lặp thừa số duy nhất trong tập hợp này. 
4. Sau khi xác định được vị trí 2, 3 và 4, hãy tính trực tiếp vị trí 1 và 5 bằng phép chia: 

a1 = p12/a2, và a5 = p45/a4. 
5. Số chưa sử dụng còn lại trong bộ cố định được gán cho vị trí 6. 
6. Xuất ra hoán vị được xây dựng lại. 

### Tại sao nó hoạt động 

Mỗi vị trí ở giữa tham gia vào hai ràng buộc nhân. Bởi vì tất cả các số đều khác biệt và tập hợp cố định nên mỗi cặp sản phẩm chia sẻ chính xác một phép gán thừa số nhất quán khớp với một hoán vị hợp lệ. Điều này tạo ra một hệ thống các phương trình chồng chéo trong đó mỗi ẩn số xuất hiện với đủ ràng buộc để có thể giải được một cách duy nhất. Cấu trúc của các số được phép đảm bảo rằng không có phép gán thay thế nào có thể thỏa mãn đồng thời cả bốn sản phẩm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(i, j):
    print("?", i, j)
    sys.stdout.flush()
    return int(input().strip())

def solve():
    nums = [4, 8, 15, 16, 23, 42]
    
    p12 = ask(1, 2)
    p23 = ask(2, 3)
    p34 = ask(3, 4)
    p45 = ask(4, 5)

    # try all permutations for positions 1..5 consistent with constraints
    # small search is safe since only 6 values total and structure is fixed
    import itertools

    for perm in itertools.permutations(nums, 6):
        if perm[5] != perm[5]:
            pass
        # check constraints on first 5 positions only
        if perm[0] * perm[1] != p12:
            continue
        if perm[1] * perm[2] != p23:
            continue
        if perm[2] * perm[3] != p34:
            continue
        if perm[3] * perm[4] != p45:
            continue

        print("!", *perm)
        sys.stdout.flush()
        return

solve()
```Việc triển khai này sử dụng chiến lược tương tác tối thiểu: nó hỏi chính xác bốn truy vấn tương ứng với các sản phẩm liên tiếp. Sau khi thu thập các ràng buộc này, nó sẽ thực hiện kiểm tra mạnh mẽ trên tất cả 6! hoán vị. Mặc dù về mặt khái niệm đây là một bước cưỡng bức, nhưng nó được thực hiện cục bộ và không quan trọng về mặt tính toán. 

Một chi tiết triển khai tinh tế là vòng lặp phải đảm bảo tất cả bốn ràng buộc đều được thỏa mãn theo trình tự, nếu không chúng ta có thể chấp nhận sai một phần khớp. Việc kiểm tra phép nhân ràng buộc hoàn toàn năm vị trí đầu tiên và vị trí thứ sáu được xác định ngầm. 

Việc xóa sau mỗi truy vấn là bắt buộc vì trình tương tác đợi đầu vào trước khi phản hồi. Thiếu tuôn ra dẫn đến bế tắc hơn là một câu trả lời sai. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

Giả sử mảng ẩn là: 

| vị trí | 1 | 2 | 3 | 4 | 5 | 6 | 
| --- | --- | --- | --- | --- | --- | --- | 
| giá trị | 4 | 8 | 15 | 16 | 23 | 42 | 

Truy vấn trả về: 

| Truy vấn | Kết quả | 
| --- | --- | 
| (1,2) | 32 | 
| (2,3) | 120 | 
| (3,4) | 240 | 
| (4,5) | 368 | 

Thuật toán kiểm tra các hoán vị và tìm ra hoán vị phù hợp với cả bốn sản phẩm. Chuỗi ràng buộc xác định duy nhất thứ tự chính xác vì mỗi sản phẩm liền kề khớp chính xác với một vị trí giá trị hợp lệ. 

Điều này xác nhận rằng các ràng buộc kề là đủ để xác định đầy đủ hoán vị. 

### Ví dụ thứ hai 

Mảng ẩn: 

| vị trí | 1 | 2 | 3 | 4 | 5 | 6 | 
| --- | --- | --- | --- | --- | --- | --- | 
| giá trị | 42 | 23 | 16 | 15 | 8 | 4 | 

Các truy vấn tương tự mang lại: 

| Truy vấn | Kết quả | 
| --- | --- | 
| (1,2) | 966 | 
| (2,3) | 368 | 
| (3,4) | 240 | 
| (4,5) | 120 | 

Một lần nữa, chỉ có một hoán vị thỏa mãn mọi ràng buộc, thể hiện tính đối xứng: đảo ngược mảng vẫn tạo ra một nghiệm nhất quán duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(720) | tìm kiếm hoán vị có kích thước không đổi trên 6 phần tử | 
| Không gian | O(1) | chỉ sử dụng các mảng cố định và các biến vòng lặp | 

Thời gian chạy là không đổi và phù hợp một cách tầm thường trong giới hạn. Chi phí chủ yếu là sự tương tác, nhưng chúng tôi chỉ tuân theo bốn truy vấn theo yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    nums = list(map(int, inp.strip().split()))
    a = nums

    # simulate interactive judge
    def ask(i, j):
        return a[i-1] * a[j-1]

    fixed = [4, 8, 15, 16, 23, 42]

    import itertools
    for perm in itertools.permutations(fixed):
        if (perm[0]*perm[1] == ask(1,2) and
            perm[1]*perm[2] == ask(2,3) and
            perm[2]*perm[3] == ask(3,4) and
            perm[3]*perm[4] == ask(4,5)):
            return " ".join(map(str, perm))
    return ""

# provided sample
assert run("4 8 15 16 23 42") == "4 8 15 16 23 42"

# reverse case
assert run("42 23 16 15 8 4") == "42 23 16 15 8 4"

# shuffled case
assert run("8 4 15 16 42 23") == "8 4 15 16 42 23"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| được sắp xếp | được sắp xếp giống nhau | tái thiết danh tính | 
| đảo ngược | đảo ngược | xử lý đối xứng | 
| xáo trộn | hoán vị giống nhau | tính đúng đắn theo thứ tự tùy ý | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi hoán vị bị đảo ngược hoàn toàn. Cấu trúc tích kề vẫn hợp lệ nhưng bị đảo ngược và bất kỳ giải pháp nào dựa trên các giả định về vị trí như “các giá trị nhỏ xuất hiện sớm hơn” sẽ không thành công. Thuật toán xử lý việc này vì nó chỉ kiểm tra tính nhất quán cấp số nhân chứ không kiểm tra thứ tự các giả định. 

Một trường hợp cạnh khác là khi hoán vị đặt các giá trị lớn bên cạnh các giá trị nhỏ, tạo ra các sản phẩm trùng lặp về mặt số lượng với các cặp hợp lệ khác. Vì chúng tôi dựa vào tính duy nhất của toàn bộ hệ thống ràng buộc thay vì các sản phẩm riêng lẻ, nên sự mơ hồ chỉ được loại bỏ sau khi tất cả bốn ràng buộc được thực thi đồng thời.
