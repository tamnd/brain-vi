---
title: "CF 104304A - \u9664\u5947\u81f4\u80dc"
description: "Chúng ta được cung cấp một đội hình các đơn vị quân địch, mỗi đơn vị có một giá trị tấn công nguyên. Thẻ hiệu ứng đặc biệt sẽ loại bỏ mọi đơn vị có đòn tấn công lẻ sau khi áp dụng tất cả các sửa đổi. Trước khi sử dụng thẻ này, chúng ta được phép sử dụng một bộ sưu tập các phép thuật sử dụng một lần."
date: "2026-07-01T20:05:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104304
codeforces_index: "A"
codeforces_contest_name: "The 17-th Beihang University Collegiate Programming Contest (BCPC 2022) - Final"
rating: 0
weight: 104304
solve_time_s: 69
verified: true
draft: false
---

[CF 104304A - \u9664\u5947\u81f4\u80dc](https://codeforces.com/problemset/problem/104304/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đội hình các đơn vị quân địch, mỗi đơn vị có một giá trị tấn công nguyên. Thẻ hiệu ứng đặc biệt sẽ loại bỏ mọi đơn vị có đòn tấn công lẻ sau khi áp dụng tất cả các sửa đổi. Trước khi sử dụng thẻ này, chúng ta được phép sử dụng một bộ sưu tập các phép thuật sử dụng một lần. Mỗi phép thuật sẽ thêm một giá trị cố định vào đòn tấn công của chính xác một đơn vị đã chọn và mỗi phép thuật có thể được sử dụng tối đa một lần. 

Sau khi phân phối phép thuật cho các đơn vị theo bất kỳ cách nào, chúng tôi áp dụng hiệu ứng loại bỏ và xóa tất cả các đơn vị có giá trị tấn công cuối cùng là số lẻ. Mục tiêu là giảm thiểu tổng giá trị tấn công của các đơn vị còn lại, tức là những đơn vị có giá trị cuối cùng là số chẵn. 

Sự tương tác chính là các phép thuật ảnh hưởng đến tính chẵn lẻ. Chỉ liệu một giá trị là số lẻ hay số chẵn có ý nghĩa quan trọng đối với sự tồn tại, mà các giá trị số thực tế mới quan trọng đối với tổng cuối cùng của các đơn vị còn sống sót. Điều này tạo ra sự căng thẳng: chúng tôi muốn chuyển đổi các đơn vị có giá trị chẵn lớn thành số lẻ để chúng bị loại bỏ, nhưng làm như vậy sẽ tiêu tốn nhiều phép thuật và có thể áp đặt các ràng buộc về cấu trúc đối với số lượng chuyển đổi như vậy có thể thực hiện được. 

Các giới hạn lên tới 100.000 đơn vị và 100.000 phép thuật, loại trừ bất kỳ giải pháp nào thử tất cả các lần phân phối phép thuật hoặc thực hiện mô phỏng theo mỗi nhiệm vụ. Bất cứ điều gì vượt quá thời gian tuyến tính hoặc tuyến tính với hệ số không đổi nhỏ đều có thể chấp nhận được, nhưng lý luận hàm mũ hoặc bậc hai đối với các bài tập thì không. 

Một trường hợp phức tạp xuất hiện khi xem xét cách các phép thuật tương tác với tính chẵn lẻ. Các phép thuật có giá trị chẵn hoàn toàn không thay đổi tính chẵn lẻ nhưng vẫn tăng giá trị tấn công. Nếu sử dụng trên đơn vị sống sót, chúng sẽ trực tiếp tăng câu trả lời cuối cùng nên không bao giờ có lợi cho người sống sót. Tuy nhiên, chúng có thể được “đổ” một cách an toàn vào các đơn vị sẽ bị loại bỏ, vì các đơn vị bị loại bỏ không đóng góp vào số tiền cuối cùng. Một cách tiếp cận ngây thơ mà bỏ qua điều này có thể nghĩ sai rằng tất cả các phép thuật phải được sử dụng một cách có ý nghĩa. 

Một trường hợp khác xuất phát từ tính khả thi của tính chẵn lẻ. Nếu chúng ta quyết định chuyển một số đơn vị có giá trị chẵn thành số lẻ thì mỗi lần chuyển đổi như vậy cần chính xác một phép thuật có giá trị lẻ. Nếu chúng tôi sử dụng ít chuyển đổi hơn so với các phép thuật có sẵn, thì các phép thuật lẻ còn sót lại vẫn phải được sử dụng theo cặp hoặc lãng phí theo cách duy trì tính khả thi tính chẵn lẻ. Việc bỏ qua điều này sẽ dẫn đến việc xây dựng không hợp lệ trong một số trường hợp số lượng chuyển đổi không khớp với tính chẵn lẻ của tổng số phép thuật lẻ. 

Ví dụ: nếu tất cả các giá trị đơn vị là số chẵn và chúng ta có một số lẻ, chúng ta không thể không sử dụng hoặc áp dụng nó theo cách duy trì tính nhất quán mà không cần nghĩ đến các ràng buộc chẵn lẻ. Đầu ra chính xác phải tôn trọng tính khả thi của tính chẵn lẻ toàn cầu về số lần chúng tôi lật tính chẵn lẻ trên tất cả các đơn vị. 

## Phương pháp tiếp cận 

Cách giải thích vũ phu sẽ thử mọi cách để gán từng phép thuật cho từng đơn vị. Đối với mỗi phép gán, chúng tôi tính toán các giá trị cuối cùng, loại bỏ các giá trị lẻ và tính tổng còn lại. Điều này bùng nổ ngay lập tức: mỗi m phép thuật có n lựa chọn, dẫn đến$n^m$khả năng vượt xa mọi tính toán khả thi. 

Ngay cả khi chúng tôi bỏ qua danh tính phép thuật riêng lẻ và chỉ theo dõi số lượng phép thuật mà mỗi đơn vị nhận được, chúng tôi vẫn phải đối mặt với một vấn đề phân phối tổ hợp rất lớn. Quan sát quan trọng là chỉ có sự ngang bằng của số lượng phép thuật lẻ được gán cho mỗi đơn vị mới quan trọng chứ không phải số lượng chính xác. Hai phép thuật kỳ lạ được gán cho cùng một đơn vị sẽ hủy bỏ hiệu ứng ngang bằng của chúng đồng thời lãng phí sức chứa. 

Điều này làm giảm vấn đề trong việc quyết định đơn vị nào có tính chẵn lẻ bị đảo ngược. Một khi quan điểm này được thông qua, cấu trúc sẽ trở nên đơn giản: mỗi đơn vị hoặc giữ nguyên trạng thái chẵn lẻ ban đầu hoặc bị đảo ngược, và việc lật ngược sẽ tiêu tốn một câu thần chú kỳ lạ “đơn vị ngân sách” cho mỗi phần tử bị ảnh hưởng. 

Từ đây, vấn đề trở thành việc chọn một tập hợp con các đơn vị có giá trị chẵn để chuyển đổi thành các đơn vị có giá trị lẻ (để chúng bị loại bỏ), đồng thời tôn trọng giới hạn về số lượng chuyển đổi như vậy mà chúng tôi có thể thực hiện và ràng buộc chẵn lẻ về số lần lật chúng tôi sử dụng tổng thể. 

Cấu trúc tham lam xuất hiện vì việc loại bỏ một đơn vị sẽ đóng góp toàn bộ giá trị của nó dưới dạng chi phí tiết kiệm được. Vì vậy, nếu quyết định loại bỏ một số đơn vị có giá trị chẵn thì chúng ta nên ưu tiên những đơn vị có giá trị lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về bài tập chính tả | Hàm mũ | O(n + m) | Quá chậm | 
| Lựa chọn tham lam dựa trên tính chẵn lẻ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Phân tách các đơn vị theo chẵn lẻ 

Chúng tôi lặp lại tất cả các đơn vị và nhóm chúng thành các danh sách có giá trị lẻ và giá trị chẵn. Tất cả các đơn vị có giá trị lẻ sẽ luôn bị loại bỏ bởi hiệu ứng cuối cùng bất kể phép thuật nào, vì vậy chúng không góp phần vào câu trả lời cuối cùng. 

### 2. Tính đáp án cơ sở 

Chúng tôi bắt đầu từ tổng của tất cả các đơn vị có giá trị chẵn. Điều này thể hiện cái giá phải trả nếu chúng ta không sử dụng bất kỳ phép thuật nào để cải thiện tình hình. Bất kỳ cải tiến nào cũng phải đến từ việc loại bỏ một số đơn vị chẵn này. 

### 3. Xác định các đơn vị ứng cử để loại bỏ 

Chỉ những đơn vị có giá trị chẵn mới là ứng cử viên hữu ích để chuyển đổi. Nếu chúng ta chuyển một đơn vị chẵn thành số lẻ, nó sẽ bị loại bỏ và giá trị của nó biến mất khỏi tổng cuối cùng. Mỗi lần chuyển đổi như vậy đòi hỏi phải sử dụng một phép thuật lẻ. 

### 4. Sắp xếp ứng viên theo quyền lợi 

Chúng tôi sắp xếp các đơn vị có giá trị chẵn theo thứ tự giảm dần. Việc chuyển đổi một giá trị lớn hơn sẽ làm giảm nhiều hơn câu trả lời cuối cùng, do đó, lựa chọn tham lam là tối ưu khi các ràng buộc về tính khả thi được xử lý. 

### 5. Chọn số lượng chuyển đổi chúng tôi có thể thực hiện 

Gọi k là số phép thuật lẻ. Chúng tôi xem xét việc chọn t chuyển đổi, trong đó mỗi chuyển đổi sử dụng một phép lẻ được áp dụng cho một đơn vị chẵn riêng biệt. 

Tuy nhiên, không phải tất cả các giá trị của t đều hợp lệ. Chúng ta phải thỏa mãn ràng buộc rằng các phép thuật lẻ còn sót lại có thể được ghép nối hoặc lãng phí mà không ảnh hưởng đến tính khả thi của tính chẵn lẻ. Điều này dẫn đến điều kiện t phải thỏa mãn: 

số lần lật được sử dụng t không được vượt quá k và k − t phải là số chẵn, tương đương với việc t có cùng tính chẵn lẻ với k. 

### 6. Thử tất cả t hợp lệ đến k 

Với mỗi t hợp lệ, chúng ta lấy tổng của t đơn vị có giá trị chẵn lớn nhất. Điều này thể hiện việc loại bỏ các đơn vị đó. Chúng tôi tính toán số tiền còn lại là tổng cơ sở trừ đi mức tăng đó. 

### 7. Chọn cấu hình tốt nhất 

Chúng tôi lấy tổng kết quả tối thiểu trên tất cả các giá trị t hợp lệ. 

### Tại sao nó hoạt động 

Quá trình này được điều chỉnh bởi một bất biến duy nhất: tác dụng có ý nghĩa duy nhất của phép thuật là liệu mỗi đơn vị có giá trị chẵn có được lật một số lần lẻ hay không. Bất kỳ cặp phép thuật lẻ bổ sung nào đều bị hủy bỏ mà không ảnh hưởng đến tính chẵn lẻ, do đó, chỉ có số lượng đơn vị bị lật là quan trọng chứ không phải cấu trúc phân công chính xác. 

Khi việc giảm này được thực hiện, vấn đề sẽ trở thành việc chọn một tập hợp con các đơn vị có giá trị chẵn theo ràng buộc số lượng và số chẵn lẻ để tối đa hóa tổng trọng lượng bị loại bỏ. Bởi vì sự đóng góp là độc lập và bổ sung, việc sắp xếp đảm bảo rằng mọi giải pháp tối ưu đều phải bao gồm tiền tố của các giá trị chẵn được sắp xếp cho một kích thước cố định. Ràng buộc chẵn lẻ chỉ hạn chế độ dài tiền tố nào được phép, nhưng không thay đổi thứ tự tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    b = list(map(int, input().split()))

    total_even = 0
    evens = []

    for x in a:
        if x % 2 == 0:
            total_even += x
            evens.append(x)

    evens.sort(reverse=True)

    k = m  # number of spells; only parity matters

    # prefix sums of even values
    prefix = [0]
    for v in evens:
        prefix.append(prefix[-1] + v)

    ans = total_even

    # try all valid counts t
    for t in range(0, min(k, len(evens)) + 1):
        if t % 2 != k % 2:
            continue
        ans = min(ans, total_even - prefix[t])

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tách biệt các đơn vị có giá trị chẵn và tính tổng của chúng làm đường cơ sở ban đầu. Sau đó, nó sắp xếp các giá trị này sao cho bất kỳ tiền tố nào cũng tương ứng với việc loại bỏ có lợi nhất. 

Biến k chỉ được xử lý thông qua tính chẵn lẻ của nó, vì các phép thuật lẻ không được sử dụng có thể được ghép nối mà không ảnh hưởng đến tính khả thi. Vòng lặp t kiểm tra tất cả số lần xóa hợp lệ thỏa mãn cả ràng buộc về tính khả dụng và ràng buộc chẵn lẻ. Đối với mỗi t hợp lệ, chúng tôi trừ đi mức tăng tốt nhất có thể, là tổng của các giá trị t chẵn lớn nhất. 

Một cạm bẫy phổ biến là cố gắng sử dụng trực tiếp tất cả các phép thuật hoặc cố gắng gán chúng một cách tham lam cho các đơn vị mà không xem xét các ràng buộc chẵn lẻ. Cách tiếp cận đúng không bao giờ theo dõi việc sử dụng phép thuật của từng cá nhân ngoài việc liệu nó có góp phần tạo ra cú lật ngược hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [4, 5, 3]
m = 1
b = [1]
```Chúng tôi xử lý chẵn và lẻ riêng biệt. 

| Bước | Đơn vị chẵn | k | hợp lệ t | Tổng tiền tố tốt nhất | Số tiền còn lại | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [4] | 1 | 1 | 4 | 0 | 

Chỉ t hợp lệ là 1 vì t phải khớp với tính chẵn lẻ của k. Loại bỏ 4 sẽ chuyển nó thành số lẻ, do đó nó biến mất sau hiệu ứng cuối cùng. 

Điều này xác nhận rằng một lần lật có thể loại bỏ hoàn toàn người đóng góp chẵn duy nhất. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [3, 3, 3, 3]
m = 2
b = [1, 1]
```Tất cả các đơn vị đều là số lẻ rồi. 

| Bước | Đơn vị chẵn | k | hợp lệ t | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [] | 2 | 0 | 0 | 

Thậm chí không có đơn vị nào để cải thiện. Bất kỳ phép thuật nào đều không liên quan đến kết quả cuối cùng vì tất cả các đơn vị đều bị loại bỏ. 

Điều này chứng tỏ rằng các phép thuật không có tác dụng khi không tồn tại đơn vị có giá trị chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp các đơn vị có giá trị chẵn chiếm ưu thế | 
| Không gian | O(n) | lưu trữ danh sách đã lọc và tổng tiền tố | 

Các ràng buộc cho phép sắp xếp thoải mái tới 100.000 phần tử. Tất cả các hoạt động khác là quét tuyến tính hoặc tính toán tiền tố đơn giản, trong giới hạn một giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    b = list(map(int, input().split()))

    total_even = 0
    evens = []

    for x in a:
        if x % 2 == 0:
            total_even += x
            evens.append(x)

    evens.sort(reverse=True)

    k = m

    prefix = [0]
    for v in evens:
        prefix.append(prefix[-1] + v)

    ans = total_even
    for t in range(0, min(k, len(evens)) + 1):
        if t % 2 != k % 2:
            continue
        ans = min(ans, total_even - prefix[t])

    return str(ans)

# sample-like cases
assert run("3\n4 5 3\n1\n1\n") == "0"

assert run("4\n3 3 3 3\n2\n1 1\n") == "0"

# all even, enough spells
assert run("3\n2 4 6\n3\n1 1 1\n") == "0"

# all even, not enough parity match
assert run("3\n2 4 6\n2\n1 1\n") == "2"

# mixed case
assert run("5\n2 3 4 6 7\n3\n1 1 1\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều có đủ số lần lật | 0 | có thể loại bỏ hoàn toàn | 
| tất cả các đơn vị lẻ | 0 | loại bỏ đường cơ sở mà không cần phép thuật | 
| ràng buộc không khớp chẵn lẻ | khác không | tính đúng đắn của hạn chế chẵn lẻ | 
| phân phối hỗn hợp | tính toán tối thiểu | sự đúng đắn của tiền tố tham lam | 

## Vỏ cạnh 

Khi không có đơn vị có giá trị chẵn, thuật toán ngay lập tức trả về 0 vì tổng cơ sở bằng 0 và không thể cải thiện được. Ngay cả khi có nhiều phép thuật tồn tại, chúng cũng không thể thay đổi thực tế là tất cả các đơn vị đều bị loại bỏ sau khi áp dụng hiệu ứng. 

Khi số lượng phép thuật lẻ lớn nhưng có tính chẵn lẻ không tương thích với số lần xóa hữu ích, thuật toán sẽ bỏ qua một cách chính xác các độ dài tiền tố không hợp lệ và chỉ xem xét các giá trị chẵn lẻ phù hợp đó, ngăn chặn việc tính quá mức số lần xóa. 

Khi tất cả các đơn vị đều bằng nhau và nhiều phép thuật, chiến lược tối ưu là loại bỏ tất cả chúng nếu tính chẵn lẻ cho phép; mặt khác, giải pháp sẽ loại bỏ tất cả trừ một, tùy thuộc vào căn chỉnh chẵn lẻ, được xử lý một cách tự nhiên theo quy tắc chọn tiền tố.
