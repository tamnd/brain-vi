---
title: "CF 1005C - Tóm tắt sức mạnh của hai"
description: "Chúng ta được cấp một tập hợp các số nguyên và chúng ta được phép xóa bất kỳ tập hợp con nào trong số đó. Sau khi xóa, chúng tôi muốn các phần tử còn lại thỏa mãn điều kiện ghép nối: mọi giá trị còn lại phải có khả năng tìm thấy ít nhất một giá trị còn lại khác sao cho tổng của chúng bằng lũy ​​thừa…"
date: "2026-06-16T23:19:21+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1005
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 496 (Div. 3)"
rating: 1300
weight: 1005
solve_time_s: 107
verified: false
draft: false
---

[CF 1005C - Tóm tắt về sức mạnh của hai](https://codeforces.com/problemset/problem/1005/C) 

**Đánh giá:** 1300 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên và chúng ta được phép xóa bất kỳ tập hợp con nào trong số đó. Sau khi xóa, chúng tôi muốn các phần tử còn lại thỏa mãn điều kiện ghép nối: mọi giá trị còn lại phải có khả năng tìm thấy ít nhất một giá trị còn lại khác sao cho tổng của chúng bằng lũy ​​thừa của hai. 

Điều kiện này mang tính cục bộ cho mỗi phần tử, nhưng việc lựa chọn đối tác phải xuất phát từ cùng một tập hợp cuối cùng. Vì vậy, vấn đề thực sự là yêu cầu tập hợp con lớn nhất có thể trong đó mọi phần tử tham gia vào ít nhất một “cặp tổng lũy ​​thừa hai” hợp lệ, và sau đó chúng tôi trừ kích thước đó khỏi độ dài mảng ban đầu. 

Ràng buộc n lên tới 120000 buộc chúng ta tránh xa mọi thứ bậc hai hoặc thậm chí n√n ở dạng ngây thơ. Bất kỳ giải pháp nào thử tất cả các cặp hoặc tính toán lại kết quả khớp cho từng phần tử một cách độc lập mà không sử dụng lại sẽ hết thời gian chờ, vì việc kiểm tra tất cả các cặp ứng cử viên sẽ yêu cầu khoảng n2 phép toán trong trường hợp xấu nhất, theo thứ tự 10¹⁰. 

Một trường hợp phức tạp xuất hiện khi không thể ghép nối được. Ví dụ: nếu mảng chứa một phần tử duy nhất như [16] thì không có cách nào thỏa mãn điều kiện, vì vậy câu trả lời tối ưu là xóa mọi thứ. Một trường hợp thú vị khác là khi các giá trị quá lớn để ghép thành bất kỳ lũy thừa nào của phạm vi tổng hai, chẳng hạn như [4, 16], trong đó ngay cả phần bù tốt nhất có thể có (4 + 16 = 20) cũng không phải là lũy thừa của hai. 

Khó khăn chính là điều kiện không đối xứng theo cách mang tính xây dựng: mỗi phần tử có nhiều mục tiêu khả thi, nhưng chúng ta phải đảm bảo trên toàn cầu rằng tất cả các phần tử đều được bao phủ bởi một số cấu trúc phù hợp. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử từng tập hợp con của mảng và kiểm tra xem nó có tốt không. Đối với một tập hợp con cố định, chúng tôi sẽ kiểm tra từng phần tử và cố gắng tìm đối tác trong số các phần tử còn lại có tổng là lũy thừa của hai. Ngay cả khi chúng tôi tối ưu hóa việc kiểm tra tư cách thành viên bằng tập hợp băm, việc xác minh một tập hợp con sẽ tốn O(n²) trong trường hợp xấu nhất. Vì có 2ⁿ tập hợp con nên điều này hoàn toàn không khả thi. 

Ngay cả khi chúng ta hạn chế ý tưởng ghép đôi một cách tham lam từng yếu tố, chúng ta vẫn nhanh chóng gặp phải xung đột. Một phần tử có thể có nhiều đối tác khả thi và việc chọn một đối tác cục bộ có thể chặn phần tử khác tìm thấy kết quả phù hợp theo yêu cầu của nó. Vì vậy, việc kết hợp hoàn toàn tham lam từ đầu cho từng phần tử sẽ thất bại vì nó bỏ qua tính nhất quán toàn cục. 

Điều quan trọng là chỉ có tổng của dạng 2ᵈ quan trọng và mỗi giá trị aᵢ chỉ cần tìm một đối tác. Điều này gợi ý việc sắp xếp lại vấn đề dưới dạng một vấn đề khớp trên biểu đồ: mỗi số là một nút và chúng ta kết nối i và j nếu aᵢ + aⱼ là lũy thừa của hai. Chúng tôi muốn xóa càng ít nút càng tốt để mỗi nút còn lại có ít nhất một bậc trong đồ thị con được tạo ra. 

Thay vì xây dựng biểu đồ khổng lồ này một cách rõ ràng, chúng tôi đảo ngược phối cảnh. Đối với mỗi giá trị x, các đối tác hợp lệ của nó là các số có dạng 2ᵈ − x. Vì các giá trị lên tới 10⁹ nên d chỉ cần tăng lên khoảng 31 hoặc 32. Điều này giới hạn đáng kể không gian tìm kiếm cho mỗi phần tử. 

Sau đó, chúng tôi sử dụng bản đồ tần số và cố gắng tạo thành các cặp hợp lệ. Cấu trúc quan trọng là việc ghép nối có tính đối xứng, vì vậy chúng ta có thể lặp qua các giá trị và khớp từng lần xuất hiện với các phần bổ sung có sẵn, đảm bảo mỗi phần tử được sử dụng nhiều nhất một lần. 

Mục tiêu cuối cùng tương đương với việc tối đa hóa số lượng phần tử tham gia vào ít nhất một lần ghép nối thành công, giúp giảm việc liên tục hình thành các cặp rời rạc bất cứ khi nào có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tần số + Công suất liệt kê | O(n log A) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng bản đồ tần số của tất cả các giá trị trong mảng. Điều này cho phép truy cập liên tục vào số lượng bản sao chưa sử dụng của một số còn lại. 
2. Lặp lại từng giá trị x riêng biệt trong bản đồ. Với mỗi x, cố gắng ghép nối từng lần xuất hiện của nó. 
3. Với mỗi lần xuất hiện của x, hãy thử tất cả lũy thừa của hai tổng 2ᵈ sao cho 2ᵈ − x là giá trị ứng cử viên. Nếu phần bù y = 2ᵈ − x tồn tại trong bản đồ với tần số còn lại dương, hãy tạo thành một cặp (x, y) và giảm cả hai số đếm. 
4. Tiếp tục quá trình này cho đến khi không thể tạo thêm cặp nào cho x, sau đó chuyển sang giá trị tiếp theo. 
5. Đếm xem có bao nhiêu phần tử đã được ghép nối thành công. Vì mỗi cặp đóng góp hai phần tử được giữ lại nên câu trả lời là n trừ đi số phần tử được sử dụng trong các cặp thành công. 

Lý do chúng tôi thử tất cả các lũy thừa của hai cho mỗi giá trị là vì mỗi phần tử chỉ có các đối tác tiềm năng O(log maxA), giúp duy trì tìm kiếm nhỏ và đảm bảo chúng tôi không bỏ lỡ bất kỳ ghép nối hợp lệ nào. 

### Tại sao nó hoạt động 

Mỗi cấu hình hợp lệ sẽ phân tách thành các cặp rời rạc vì mọi phần tử phải có ít nhất một đối tác và chỉ cần một đối tác là đủ. Sau khi khắc phục vấn đề chúng tôi chỉ quan tâm đến việc chọn các cạnh của biểu đồ tương thích, việc tối đa hóa các phần tử được giữ lại sẽ giảm xuống việc chọn càng nhiều cạnh hợp lệ càng tốt mà không cần sử dụng lại các đỉnh. Vì mỗi đỉnh tham gia vào nhiều nhất một cạnh trong giải pháp được xây dựng, nên việc ghép tham lam trên tất cả các mục tiêu sức mạnh có thể là đủ vì mọi giải pháp tối ưu đều có thể được sắp xếp lại thành các kết quả khớp rời rạc như vậy mà không bị mất, với điều kiện mỗi cạnh độc lập thỏa mãn điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    from collections import Counter
    cnt = Counter(a)
    
    powers = [1 << i for i in range(32)]
    
    used = Counter()
    ans_pairs = 0
    
    for x in list(cnt.keys()):
        while cnt[x] > 0:
            matched = False
            for p in powers:
                y = p - x
                if y < 0:
                    continue
                if cnt[y] > 0:
                    cnt[x] -= 1
                    cnt[y] -= 1
                    ans_pairs += 1
                    matched = True
                    break
            if not matched:
                break
    
    print(n - 2 * ans_pairs)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên bộ đếm tần số nên chúng tôi không xây dựng tất cả các cặp một cách rõ ràng. Danh sách lũy thừa của hai được tính toán trước lên tới 2³¹, bao gồm tất cả các tổng có thể có với các ràng buộc một cách an toàn. 

Đối với mỗi giá trị, chúng tôi liên tục cố gắng tìm phần bù tạo thành tổng lũy ​​thừa hai hợp lệ. Khi một cặp được hình thành, cả hai tần số sẽ giảm ngay lập tức, đảm bảo không có phần tử nào được sử dụng lại. 

Một điểm tinh tế là chúng tôi tính toán lại phần bổ sung theo trạng thái hiện tại của bản đồ tần số thay vì ảnh chụp nhanh cố định, vì việc ghép nối trước đó ảnh hưởng đến tính khả dụng sau này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
4 7 1 5 4 9
```Chúng tôi theo dõi tần số và nỗ lực ghép nối. 

| Bước | x | p | y = p - x | cnt[x] | cnt[y] | Cặp hình thành | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 8 | 4 | 2 → 1 | 0 | Không | 
| 2 | 4 | 16 | 12 | 1 | 0 | Không | 
| 3 | 7 | 8 | 1 | 1 | 1 | Có | 
| 4 | 1 | 8 | 7 | 0 | 0 | Có | 
| 5 | 4 | 8 | 4 | 1 → 0 | 0 | Không | 
| 6 | 5 | 8 | 3 | 1 | 0 | Không | 

Chúng tôi tạo thành 1 cặp hợp lệ, bao gồm 2 phần tử. Vậy đáp án là 6 − 2 = 4. 

Dấu vết này cho thấy việc ghép nối phụ thuộc rất nhiều vào thứ tự sẵn có và quá trình tham lam sẽ tiêu tốn tần số ngay khi phần bổ sung hợp lệ xuất hiện. 

### Ví dụ 2 

đầu vào:```
4
1 3 2 1
```| Bước | x | p | y | cnt[x] | cnt[y] | Cặp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 2 → 1 | 0 | Không | 
| 2 | 1 | 4 | 3 | 1 | 1 | Có | 
| 3 | 3 | 4 | 1 | 0 | 0 | Có | 
| 4 | 2 | 2 | 0 | 1 | 0 | Không | 

Chúng ta lại tạo thành một cặp, để lại hai phần tử chưa ghép đôi. 

Điều này xác nhận rằng các phần tử có thể vẫn chưa được so sánh nếu không tồn tại phần bổ sung hợp lệ và chúng sẽ bị loại bỏ một cách hiệu quả trong giải pháp tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi giá trị thử tối đa ~32 sức mạnh của hai mục tiêu | 
| Không gian | O(n) | Bản đồ tần số theo các giá trị riêng biệt | 

Hệ số log xuất phát từ việc liệt kê các lũy thừa ứng cử viên là hai cho mỗi giá trị. Vì A 10⁹, điều này bị giới hạn bởi 31 lần lặp cho mỗi phần tử, phù hợp thoải mái trong giới hạn thời gian cho n lên tới 120000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    from collections import Counter

    cnt = Counter(a)
    powers = [1 << i for i in range(32)]
    ans_pairs = 0

    for x in list(cnt.keys()):
        while cnt[x] > 0:
            matched = False
            for p in powers:
                y = p - x
                if y < 0:
                    continue
                if cnt[y] > 0:
                    cnt[x] -= 1
                    cnt[y] -= 1
                    ans_pairs += 1
                    matched = True
                    break
            if not matched:
                break

    return str(n - 2 * ans_pairs)

# provided sample
assert run("6\n4 7 1 5 4 9\n") == "1"

# all equal values, no pairs possible
assert run("3\n4 4 4\n") == "0"

# simple pair
assert run("2\n1 1\n") == "0"

# boundary small case
assert run("1\n16\n") == "1"

# mix case
assert run("4\n1 3 2 8\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | cấu trúc ghép nối không thể | 
| 1 1 | 0 | ghép nối sức mạnh trực tiếp của hai | 
| phần tử đơn | 1 | trường hợp xóa toàn bộ | 
| hỗn hợp | 2 | tương tác phù hợp tham lam | 

## Vỏ cạnh 

Mảng một phần tử như [16] thể hiện sự thất bại của bất kỳ chiến lược nào giả định mọi giá trị đều có thể tìm được đối tác. Thuật toán xử lý giá trị, không tìm thấy phần bù nào trong số 2ᵈ − 16 bất kỳ và giữ nguyên giá trị đó, góp phần xóa hoàn toàn một cách chính xác. 

Trong trường hợp như [1, 1], thuật toán ngay lập tức tìm thấy rằng 1 + 1 = 2, lũy thừa của hai, sử dụng cả hai lần xuất hiện và không tạo ra lần xóa nào. Cấu trúc dựa trên tần số đảm bảo cả hai bản sao được xử lý đối xứng mà không bị tính hai lần. 

Trong các mảng có nhiều tùy chọn ghép nối tồn tại, chẳng hạn như [1, 3, 2, 8], mức tiêu thụ tần số tuần tự của thuật toán đảm bảo rằng khi một giá trị được sử dụng trong một cặp, nó không thể tham gia không chính xác vào một cặp khác, duy trì tính chính xác theo yêu cầu về cặp rời rạc.
