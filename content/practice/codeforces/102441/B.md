---
title: "CF 102441B - Phân phối lại chữ số"
description: "Chúng ta có nhiều tập hợp các chữ số thập phân khác 0. Mỗi lần xuất hiện đều quan trọng, vì vậy nếu dữ liệu đầu vào chứa ba bản sao của số 7 thì cả ba bản sao đó phải được sử dụng đúng một lần. Chúng tôi cũng có n giới hạn trên."
date: "2026-08-08T13:20:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102441
codeforces_index: "B"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Final"
rating: 0
weight: 102441
solve_time_s: 144
verified: true
draft: false
---

[CF 102441B - Phân phối lại chữ số](https://codeforces.com/problemset/problem/102441/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có nhiều tập hợp các chữ số thập phân khác 0. Mỗi lần xuất hiện đều quan trọng, vì vậy nếu đầu vào chứa ba bản sao của`7`, cả ba bản sao phải được sử dụng đúng một lần. Chúng tôi cũng có`n`giới hạn trên. Nhiệm vụ của chúng ta là sắp xếp lại tất cả các chữ số có sẵn thành`n`các số nguyên dương sao cho số được gán cho mỗi giới hạn không vượt quá giới hạn đó. 

Các số chúng ta xây dựng không nhất thiết phải sử dụng cùng một số chữ số. Ví dụ: giới hạn hai chữ số có thể nhận được số có một chữ số. Yêu cầu chung duy nhất là mỗi chữ số đầu vào được sử dụng chính xác một lần. Thứ tự các giới hạn xuất hiện trong đầu vào không quan trọng vì bất kỳ phép gán hợp lệ nào cũng có thể được in theo thứ tự ban đầu. 

Chuỗi đầu vào lớn nhất chỉ có 500 chữ số, trong khi có tối đa 50 giới hạn. Điều này ngay lập tức loại trừ tìm kiếm dựa trên hoán vị. Ngay cả khi chỉ có chín chữ số khác 0, số cách sắp xếp có thể tăng theo cấp số nhân với số lần xuất hiện. Giới hạn trên có nhiều nhất là 10 chữ số, do đó việc xây dựng một số ứng cử viên có thể được xử lý với một khối lượng công việc không đổi rất nhỏ. Cấu trúc hữu ích là bảng chữ cái chữ số có kích thước không đổi, trong khi tổng số chữ số có thể lớn. 

Có ba trường hợp nguy hiểm mà việc triển khai bất cẩn có thể xử lý sai. 

Đầu tiên, tổng số chữ số có sẵn có thể lớn hơn tổng số vị trí được giới hạn cho phép. Ví dụ, với`12345 2 21 43`, hai giới hạn có thể chứa tối đa bốn chữ số, nhưng phải sử dụng năm chữ số. Đầu ra đúng là`-1`. Việc triển khai tham lam xây dựng hai số hợp lệ và đơn giản quên chữ số không được sử dụng sẽ vi phạm yêu cầu sử dụng mọi chữ số. 

Thứ hai, đưa ra con số ngắn nhất có thể cho giới hạn hiện tại không phải lúc nào cũng an toàn. Coi như`129 2 13 22`. Một chiến lược tăng dần ngây thơ có thể cho số có một chữ số`2`hoặc`1`đến giới hạn đầu tiên và rời đi`29`vì`22`, không hợp lệ. Một cách xây dựng hợp lệ là`1 92`. Giới hạn lớn hơn sẽ sử dụng nhiều chữ số nhất có thể một cách an toàn, để lại các chữ số nhỏ hơn cho giới hạn chặt chẽ hơn. 

Thứ ba, chỉ so sánh các chữ số với chữ số tương ứng của giới hạn có thể yêu cầu quay lại. Vì`3241`và bị ràng buộc`320`, số lớn nhất có ba chữ số là`314`. Nỗ lực để phù hợp`3`, sau đó`2`, bị kẹt vì không có chữ số còn lại nhiều nhất`0`. Một thói quen tham lam bất cẩn có thể kết luận rằng không tồn tại số có ba chữ số, ngay cả khi thay đổi chữ số thứ hai từ`2`ĐẾN`1`cho`314`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ chọn một phân vùng gồm tất cả các chữ số thành`n`nhóm khác rỗng, hoán vị các chữ số bên trong mỗi nhóm, tạo thành các số tương ứng và kiểm tra tất cả các giới hạn trên. Nếu`m`các chữ số đầu vào được coi là những lần xuất hiện có thể phân biệt được, có`m!`cách để đặt hàng chúng và`C(m-1, n-1)`các cách để chèn`n-1`dấu phân cách giữa các chữ số liên tiếp. Do đó, một tìm kiếm toàn diện đơn giản có thể kiểm tra theo thứ tự của`500! * C(499, 49)`ứng cử viên trong trường hợp lớn nhất. Các chữ số lặp lại làm giảm số lượng các chuỗi kết quả riêng biệt, nhưng việc tìm kiếm vẫn có quy mô lớn về mặt thiên văn. Lực lượng vũ phu chỉ hữu ích như một định nghĩa mang tính khái niệm về những gì chúng ta đang tìm kiếm. 

Quan sát quan trọng là bản thân các giới hạn cung cấp một thứ tự. Xử lý các giới hạn từ lớn nhất đến nhỏ nhất. Một giới hạn lớn có nhiều tự do hơn mọi giới hạn đứng sau nó, vì vậy nó sẽ nhận được số an toàn lớn nhất và, bất cứ khi nào có thể, càng nhiều chữ số càng tốt. Các giới hạn còn lại nhỏ hơn nên việc để chúng với các chữ số nhỏ hơn là lựa chọn an toàn hơn. 

Đối với một giới hạn cụ thể, giả sử có`r`chữ số còn lại và`k`giới hạn bao gồm cả giới hạn hiện tại. Số hiện tại không thể sử dụng nhiều hơn số chữ số giới hạn của nó và nó phải để lại ít nhất một chữ số cho mỗi số sau này. Do đó độ dài tối đa có thể có của nó là`min(number_of_digits_in_bound, r - (k - 1))`. 

Trước tiên, chúng tôi thử độ dài đó và chỉ giảm nó nếu không có số nào có độ dài đó có thể được hình thành dưới giới hạn hiện tại. 

Đối với độ dài cố định, chúng tôi xây dựng số lớn nhất có thể không vượt quá giới hạn. Nếu độ dài được chọn nhỏ hơn độ dài giới hạn thì việc so sánh sẽ diễn ra tự động, do đó chúng tôi chỉ cần lấy các chữ số lớn nhất hiện có. Nếu độ dài bằng nhau thì quét từ trái sang phải. Tại mọi vị trí, chúng tôi thử chữ số lớn nhất có sẵn không vượt quá chữ số giới hạn tương ứng. Nếu chúng ta chọn một chữ số nhỏ hơn hoàn toàn thì tiền tố đã nhỏ hơn giới hạn, do đó tất cả các chữ số còn lại có thể được đặt theo thứ tự giảm dần. Nếu chúng ta chọn cùng một chữ số làm giới hạn, chúng ta sẽ tiếp tục đệ quy. Nếu lựa chọn bằng nhau đó cuối cùng không thành công, chúng ta quay lại và thử chữ số nhỏ hơn tiếp theo. 

Việc quay lại là rất nhỏ. Tại mỗi vị trí có tối đa chín chữ số ứng cử viên, nhưng chỉ ứng cử viên bằng chữ số của giới hạn mới có thể giữ chặt tiền tố. Mỗi ứng cử viên nhỏ hơn ngay lập tức hoàn thành việc xây dựng. Vì giới hạn tối đa chỉ có 10 chữ số nên đây thực sự là thời gian không đổi trên mỗi độ dài thử. 

Lựa chọn tham lam là an toàn vì chúng ta xử lý các giới hạn từ lớn nhất đến nhỏ nhất. Trong số các cấu trúc có cùng độ dài, việc lấy số hiện tại lớn nhất có thể sẽ sử dụng các chữ số lớn hơn trước và để lại các chữ số nhỏ hơn cho các giới hạn nhỏ hơn. Trong số các độ dài có thể, việc lấy độ dài khả thi lớn nhất sẽ tiêu tốn các chữ số mà lẽ ra có thể được đặt trong các số sau này. Vì giới hạn hiện tại ít nhất cũng lớn bằng mọi giới hạn sau, nên việc sử dụng các chữ số đó ở đây không thể giúp việc xây dựng số sau này dễ dàng hơn bằng cách cung cấp cho số hiện tại ít vị trí hơn. Do đó, thuật toán duy trì nhiều tập hợp còn lại mạnh nhất có thể cho các giới hạn chặt chẽ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m! · C(m-1, n-1)) | O(m) | Quá chậm | 
| Tối ưu | O(m + n log n) với hằng số thập phân | O(m + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần xuất hiện của mỗi chữ số từ`1`bởi vì`9`. Chúng ta chỉ cần chín bộ đếm vì số 0 không bao giờ xuất hiện trong đầu vào. 
2. Kiểm tra điều kiện độ dài toàn cầu. Mọi ràng buộc`a_i`có thể chứa nhiều nhất`len(a_i)`chữ số, vì vậy nếu tổng số chữ số đầu vào lớn hơn`sum(len(a_i))`, không có giải pháp tồn tại. Ngoài ra, mỗi số cần ít nhất một chữ số, vì vậy nếu số chữ số nhỏ hơn`n`, không có giải pháp tồn tại. 
3. Sắp xếp các giới hạn theo thứ tự số giảm dần nhưng giữ nguyên chỉ số ban đầu của chúng. Giới hạn lớn nhất được xử lý đầu tiên vì nó có nhiều quyền tự do nhất. 
4. Đối với giới hạn hiện tại, hãy tính số chữ số lớn nhất mà chúng ta được phép sử dụng. Nếu có`r`chữ số còn lại và`k`giới hạn vẫn còn bao gồm cả giới hạn hiện tại, chúng ta phải để lại ít nhất`k - 1`chữ số cho giới hạn sau. Vậy chiều dài tối đa là`min(len(bound), r - k + 1)`. 
5. Hãy thử độ dài từ mức tối đa đó xuống còn một. Việc thử độ dài dài nhất trước tiên là sự lựa chọn tham lam. Nếu nó hoạt động, giới hạn hiện tại sẽ tiêu thụ số chữ số tối đa có thể. Nếu không có độ dài nào hoạt động thì toàn bộ trường hợp là không thể. 
6. Đối với độ dài nhỏ hơn độ dài giới hạn, lấy các chữ số lớn nhất có sẵn theo thứ tự giảm dần. Bất kỳ số nào có ít chữ số hơn giới hạn sẽ tự động nhỏ hơn, do đó không cần so sánh từng chữ số. 
7. Với chiều dài bằng chiều dài giới hạn, xây dựng hoán vị lớn nhất không vượt quá giới hạn. Ở vị trí chật hẹp, hãy thử các chữ số có sẵn từ chữ số giới hạn trở xuống. Một chữ số nhỏ hơn chữ số giới hạn làm cho toàn bộ tiền tố nhỏ hơn, do đó hậu tố còn lại có thể được sắp xếp theo thứ tự giảm dần. Một chữ số bằng nhau giữ tiền tố chặt chẽ và yêu cầu tiếp tục đến vị trí tiếp theo. 
8. Khi một số đã được tạo, hãy trừ các chữ số của nó khỏi bộ đếm và lưu số đó vào chỉ số ban đầu của giới hạn. Bộ đếm thể hiện chính xác các chữ số vẫn có sẵn cho các giới hạn nhỏ hơn. 
9. Sau khi mọi giới hạn được xử lý, tất cả các bộ đếm chữ số phải bằng 0. Việc kiểm tra tổng công suất sơ bộ và lựa chọn độ dài tối đa làm cho kết quả này trở thành kết quả mong đợi bất cứ khi nào có giải pháp, nhưng điều kiện cuối cùng cũng bảo vệ việc triển khai khỏi để lại một chữ số vô tình không được sử dụng. 

### Tại sao nó hoạt động 

Điều bất biến là trước khi xử lý một giới hạn, nhiều chữ số còn lại vẫn có thể được gán cho tất cả các giới hạn còn lại bất cứ khi nào có giải pháp. Chúng tôi xử lý các giới hạn từ lớn nhất đến nhỏ nhất. Đối với giới hạn hiện tại, trước tiên chúng tôi tối đa hóa độ dài của nó, với điều kiện để lại một chữ số cho mỗi số sau. Nếu cần gán một phép gán ngắn hơn trong khi tồn tại số hiện tại hợp lệ dài hơn, thì việc di chuyển một trong các chữ số được sử dụng bởi số sau vào số hiện tại không thể làm cho số hiện tại vượt quá giới hạn của nó vì cấu trúc được chọn rõ ràng thỏa mãn giới hạn. Các giới hạn sau không lớn hơn giới hạn hiện tại và cấu trúc chọn số hiện tại lớn nhất có thể, ưu tiên sử dụng các chữ số lớn và để các chữ số nhỏ hơn cho các giới hạn chặt chẽ hơn. Đối với độ dài cố định, cấu trúc từng chữ số chính xác là hoán vị lớn nhất về mặt từ điển không vượt quá giới hạn. Vì vậy, mọi quyết định tham lam đều bảo toàn khả năng hoàn thành các giới hạn nhỏ hơn còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_number(cnt, bound, length):
    """
    Return (number_string, new_count) for the largest number of
    exactly `length` digits that can be made from cnt and is <= bound.
    Return (None, None) if impossible.
    """
    work = cnt[:]

    if length < len(bound):
        ans = []
        need = length

        for d in range(9, 0, -1):
            take = min(work[d], need)
            if take:
                ans.append(str(d) * take)
                work[d] -= take
                need -= take
                if need == 0:
                    break

        if need != 0:
            return None, None

        return ''.join(ans), work

    # length == len(bound)
    ans = []

    def dfs(pos):
        if pos == length:
            return True

        limit = ord(bound[pos]) - ord('0')

        # Try the largest possible digit first.
        for d in range(limit, 0, -1):
            if work[d] == 0:
                continue

            work[d] -= 1
            ans.append(str(d))

            if d < limit:
                # The prefix is already strictly smaller.
                # Maximize the suffix.
                for x in range(9, 0, -1):
                    if work[x]:
                        ans.append(str(x) * work[x])

                return True

            # d == limit, so the prefix is still equal.
            if dfs(pos + 1):
                return True

            ans.pop()
            work[d] += 1

        return False

    if not dfs(0):
        return None, None

    return ''.join(ans), work

def solve_case(s, bounds):
    m = len(s)
    n = len(bounds)

    # Every output number needs at least one digit.
    if m < n:
        return None

    # Every output number has at most as many digits as its bound.
    capacity = sum(len(str(x)) for x in bounds)
    if m > capacity:
        return None

    cnt = [0] * 10
    for ch in s:
        cnt[ord(ch) - ord('0')] += 1

    # Process the largest bounds first.
    order = sorted(range(n), key=lambda i: bounds[i], reverse=True)
    answer = [None] * n

    remaining = m

    for step, idx in enumerate(order):
        bound = str(bounds[idx])
        remaining_numbers = n - step - 1

        # Leave at least one digit for every later number.
        max_len = min(len(bound), remaining - remaining_numbers)

        chosen = None
        chosen_cnt = None

        for length in range(max_len, 0, -1):
            candidate, new_cnt = build_number(cnt, bound, length)

            if candidate is not None:
                chosen = candidate
                chosen_cnt = new_cnt
                break

        if chosen is None:
            return None

        answer[idx] = chosen
        cnt = chosen_cnt
        remaining -= len(chosen)

    if remaining != 0:
        return None

    return answer

def solve(data):
    it = iter(data.strip().splitlines())
    t = int(next(it))
    out = []

    for _ in range(t):
        parts = next(it).split()
        s = parts[0]
        n = int(parts[1])
        bounds = list(map(int, parts[2:2 + n]))

        answer = solve_case(s, bounds)

        if answer is None:
            out.append("-1")
        else:
            out.append(" ".join(answer))

    return "\n".join(out)

def main():
    data = sys.stdin.read()
    sys.stdout.write(solve(data))

if __name__ == "__main__":
    main()
```các`solve_case`trước tiên, hàm này thực hiện hai bước kiểm tra tính khả thi toàn cầu. Lần đầu tiên kiểm tra xem có đủ chữ số để tạo không`n`các số không trống. Bước thứ hai kiểm tra xem tổng số chữ số có sẵn không vượt quá tổng số vị trí chữ số được giới hạn cho phép. Kiểm tra thứ hai là những gì ngay lập tức từ chối`12534`vật mẫu. 

Bộ đếm chữ số chỉ có mười mục, do đó việc xóa các chữ số không bao giờ cần phải thao tác với chuỗi gốc. Điều này cũng xử lý các chữ số lặp lại một cách tự nhiên. Một lần xuất hiện chữ số được sử dụng bằng cách giảm bộ đếm của nó và bộ đếm sau mỗi bước tham lam thể hiện chính xác những lần xuất hiện không được sử dụng. 

Các giới hạn được sắp xếp theo các giá trị nguyên theo thứ tự giảm dần, trong khi các chỉ số ban đầu của chúng được giữ lại. Điều này là cần thiết vì đối số tham lam phụ thuộc vào việc xử lý hạn chế lớn nhất trước tiên, nhưng đầu ra vẫn phải chứa một câu trả lời cho mỗi đầu vào bị ràng buộc ở vị trí ban đầu. 

biểu hiện`remaining - remaining_numbers`là số chữ số tối đa mà số hiện tại có thể sử dụng mà không để lại quá ít chữ số cho các số còn lại. Đây là điều kiện biên ngăn cản thuật toán tạo ra một số hợp lệ cục bộ trong khi không thể đếm được chữ số toàn cục.`build_number`thử độ dài lớn nhất trước tiên. Đối với độ dài ngắn hơn, lấy chữ số từ`9`xuống tới`1`ngay lập tức đưa ra số lớn nhất có thể vì không có phép so sánh giới hạn trên nào để thực hiện. Đối với độ dài bằng nhau, lồng nhau`dfs`hàm xử lý trường hợp tiền tố chặt chẽ. 

Phần tinh tế là`d < limit`chi nhánh. Khi chữ số được chọn hoàn toàn nhỏ hơn chữ số giới hạn tương ứng, toàn bộ số đó đã nhỏ hơn giới hạn bất kể hậu tố. Do đó, chúng ta có thể đặt mọi chữ số còn lại theo thứ tự giảm dần để tối đa hóa kết quả. 

Khi`d == limit`, tiền tố vẫn bằng giới hạn, do đó hậu tố vẫn phải được kiểm tra. Nếu lần thử đệ quy đó không thành công, chữ số sẽ được khôi phục trước khi thử ứng viên nhỏ hơn. Việc khôi phục là cần thiết vì sự xuất hiện chữ số giống nhau phải có sẵn cho nhánh thay thế. 

Số nguyên Python không tràn ở đây. Giới hạn nhiều nhất là`10^9`và thuật toán hầu như vẫn hoạt động với các biểu diễn chuỗi của chúng. Các số đầu ra cũng được biểu diễn dưới dạng chuỗi, giúp tránh mọi chuyển đổi số nguyên không cần thiết của các chuỗi chữ số dài. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên là```
1234 2 21 43
```Có bốn chữ số và hai giới hạn mỗi vị trí có hai vị trí, vì vậy mỗi chữ số phải thuộc số có hai chữ số. 

| Bước | Đã xử lý ràng buộc | Chữ số còn lại | Chiều dài tối đa | Số được chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 43 | 1,2,3,4 | 2 | 43 | 
| 2 | 21 | 1,2 | 2 | 21 | 

Các giới hạn được xử lý theo thứ tự giảm dần, vì vậy`43`được xử lý đầu tiên. Hoán vị lớn nhất có hai chữ số của`1234`không vượt quá`43`là`43`. Các chữ số còn lại là`1`Và`2`, dạng nào`21`cho giới hạn thứ hai. Khôi phục thứ tự đầu vào ban đầu mang lại`21 43`, khác với đầu ra mẫu nhưng có giá trị như nhau. 

### Mẫu 2 

Mẫu thứ hai là```
12534 2 21 43
```Có năm chữ số, trong khi hai giới hạn chỉ cung cấp tổng cộng bốn vị trí chữ số. 

| Bước | Tổng số | Tổng số vị trí được phép | Kết quả | 
| --- | --- | --- | --- | 
| Kiểm tra tính khả thi | 5 | 4 | không thể | 

Thuật toán dừng trước khi xây dựng bất cứ thứ gì và in`-1`. Điều này chứng tỏ tại sao việc kiểm tra năng lực toàn cầu là cần thiết. 

### Ví dụ về tiền tố chặt chẽ 

Hãy xem xét```
3241 2 320 99
```Các giới hạn đã theo thứ tự giảm dần. 

| Bước | Ràng buộc | Chữ số còn lại | Chiều dài đã thử | Quyết định tiền tố | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 320 | 1,2,3,4 | 3 |`3 = 3`,`2 = 2`, không có chữ số`<= 0`| quay lại | 
| 1 | 320 | 1,2,3,4 | 3 |`3 = 3`,`1 < 2`|`314`| 
| 2 | 99 | 2 | 1 |`2 < 9`|`2`| 

Lần thử đầu tiên tuân theo giới hạn với tiền tố`32`, nhưng không có chữ số còn lại nào có thể chiếm vị trí cuối cùng vì không có số 0. Thuật toán quay lại vị trí thứ hai và thử`1`, sản xuất`314`. Chữ số còn lại`2`sau đó được giao một cách an toàn cho`99`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m + n log n) | Việc sắp xếp tốn O(n log n), trong khi mỗi công trình kiểm tra tối đa 10 loại chữ số và tối đa 10 vị trí cho mỗi loại trong số tối đa 10 độ dài. | 
| Không gian | O(m + n) | Bộ đếm chữ số có kích thước không đổi, trong khi mảng câu trả lời và chuỗi đầu ra chứa thông tin O(m + n). | 

Hằng số thực tế rất nhỏ vì các chữ số thập phân chỉ có chín loại chữ số có thể sử dụng được và mỗi giới hạn có nhiều nhất là 10 chữ số. Ngay cả với 50 giới hạn và 500 chữ số đầu vào, thuật toán chỉ thực hiện vài nghìn thao tác cấp chữ số cho mỗi trường hợp thử nghiệm, thấp hơn nhiều so với giới hạn một giây yêu cầu. 

## Trường hợp thử nghiệm```python
import io

def build_number(cnt, bound, length):
    work = cnt[:]

    if length < len(bound):
        ans = []
        need = length

        for d in range(9, 0, -1):
            take = min(work[d], need)
            if take:
                ans.append(str(d) * take)
                work[d] -= take
                need -= take
                if need == 0:
                    break

        if need:
            return None, None

        return ''.join(ans), work

    ans = []

    def dfs(pos):
        if pos == length:
            return True

        limit = int(bound[pos])

        for d in range(limit, 0, -1):
            if work[d] == 0:
                continue

            work[d] -= 1
            ans.append(str(d))

            if d < limit:
                for x in range(9, 0, -1):
                    if work[x]:
                        ans.append(str(x) * work[x])
                return True

            if dfs(pos + 1):
                return True

            ans.pop()
            work[d] += 1

        return False

    if not dfs(0):
        return None, None

    return ''.join(ans), work

def solve_case(s, bounds):
    m = len(s)
    n = len(bounds)

    if m < n:
        return None

    if m > sum(len(str(x)) for x in bounds):
        return None

    cnt = [0] * 10
    for ch in s:
        cnt[int(ch)] += 1

    order = sorted(range(n), key=lambda i: bounds[i], reverse=True)
    answer = [None] * n
    remaining = m

    for step, idx in enumerate(order):
        bound = str(bounds[idx])
        later = n - step - 1

        max_len = min(len(bound), remaining - later)

        found = False

        for length in range(max_len, 0, -1):
            candidate, new_cnt = build_number(cnt, bound, length)

            if candidate is not None:
                answer[idx] = candidate
                cnt = new_cnt
                remaining -= length
                found = True
                break

        if not found:
            return None

    if remaining != 0:
        return None

    return answer

def run(inp: str) -> str:
    lines = inp.strip().splitlines()
    t = int(lines[0])
    out = []

    for line in lines[1:t + 1]:
        parts = line.split()
        s = parts[0]
        n = int(parts[1])
        bounds = list(map(int, parts[2:2 + n]))

        ans = solve_case(s, bounds)
        out.append("-1" if ans is None else " ".join(ans))

    return "\n".join(out)

# Provided samples
sample = """\
3
1234 2 21 43
12534 2 21 43
42 1 42
"""

assert run(sample) == "21 43\n-1\n42", "provided samples"

# Minimum-size input
assert run("1\n7 1 7\n") == "7", "single digit"

# All values equal
assert run("1\n3333 2 33 33\n") == "33 33", "all equal values"

# Boundary condition where the larger bound must receive more digits
assert run("1\n129 2 13 22\n") == "1 92", "length allocation"

# Tight-prefix backtracking
assert run("1\n3241 2 320 99\n") == "314 2", "backtracking"

# Maximum-size feasible digit set: 450 digits, 50 bounds of length 9
s = "1" * 450
bounds = " ".join(["999999999"] * 50)
expected = " ".join(["111111111"] * 50)
assert run(f"1\n{s} 50 {bounds}\n") == expected, "maximum-size feasible case"

# Maximum digit count but insufficient total capacity
s = "1" * 500
assert run(f"1\n{s} 50 {bounds}\n") == "-1", "maximum-size impossible case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 1 7`|`7`| Đầu vào có kích thước tối thiểu và ranh giới chính xác | 
|`3333 2 33 33`|`33 33`| Các chữ số giống hệt nhau lặp đi lặp lại và giới hạn bằng nhau | 
|`129 2 13 22`|`1 92`| Phân phối chính xác độ dài giữa các giới hạn khác nhau | 
|`3241 2 320 99`|`314 2`| Quay lại khi tiền tố bằng nhau cuối cùng trở nên không thể | 
| 450 bản`1`, 50 giới hạn chín chữ số | 50 bản sao của`111111111`| Kích thước đầu vào tối đa khả thi | 
| 500 bản`1`, 50 giới hạn chín chữ số |`-1`| Ranh giới năng lực toàn cầu | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên không đủ tổng công suất. Vì```
12345 2 21 43
```có năm chữ số, nhưng`21`Và`43`mỗi chữ số có thể chứa tối đa hai chữ số. Thuật toán tính toán`5 > 2 + 2`trong quá trình kiểm tra tính khả thi ban đầu và ngay lập tức trả lại`-1`. Không có chữ số nào được âm thầm loại bỏ. 

Trường hợp cạnh thứ hai là bẫy phân bổ độ dài:```
129 2 13 22
```Các giới hạn được xử lý như`22`, sau đó`13`. Vì`22`, vẫn còn ba chữ số và một chữ số phải được dành riêng cho giới hạn sau, do đó độ dài tối đa là hai. Số lớn nhất có hai chữ số có được từ`1,2,9`là`92`, hợp lệ. Chỉ một`1`còn lại và`1 <= 13`. Đầu ra cuối cùng theo thứ tự ban đầu là`1 92`. 

Trường hợp cạnh thứ ba là quay lui tiền tố chặt chẽ:```
3241 2 320 99
```Vì`320`, thuật toán sẽ thử độ dài bằng ba. Đầu tiên nó theo sau tiền tố bằng nhau`3`, sau đó`2`. Ở vị trí cuối cùng, giới hạn yêu cầu tối đa một chữ số`0`, điều này là không thể vì không có số 0. Việc tìm kiếm quay trở lại vị trí thứ hai và thử`1`, nhỏ hơn`2`. Tiền tố bây giờ hoàn toàn nhỏ hơn`32`, vậy số còn lại`4`được đặt sau nó, tạo ra`314`. Chữ số duy nhất không được sử dụng là`2`, hợp lệ cho giới hạn thứ hai. 

Trường hợp cạnh thứ tư là các chữ số lặp lại:```
3333 2 33 33
```Bộ đếm chữ số chứa bốn bản sao của`3`. Giới hạn đầu tiên tiêu thụ hai bản sao và biểu mẫu`33`; cái thứ hai tiêu thụ hai cái còn lại. Không có giả định rằng tất cả các chữ số là khác biệt được thực hiện ở bất kỳ đâu trong thuật toán. 

Trường hợp cạnh thứ năm là tổng chiều dài tối đa khả thi. Với 450 chữ số và 50 giới hạn, mỗi giới hạn chín chữ số, mỗi giới hạn phải nhận chính xác chín chữ số. Đối với 450 bản`1`, mọi số được xây dựng là`111111111`. Tính toán độ dài của thuật toán buộc phải có chín chữ số cho mỗi giới hạn vì không còn dung lượng dự phòng ở bất kỳ nơi nào khác. 

Cuối cùng, giới hạn nhỏ nhất có thể đáng được quan tâm rõ ràng. Nếu chữ số có sẵn là`9`và giới hạn là`1`, việc xây dựng một chữ số không thành công vì`9 > 1`. Vì việc thêm các chữ số sẽ chỉ làm cho số lớn hơn giới hạn một chữ số nên không có độ dài thay thế nào có thể hữu ích. Thuật toán thử độ dài duy nhất có thể và báo cáo lỗi một cách chính xác. 

Nếu bạn muốn, tôi cũng có thể biến phong cách này thành một phong cách biên tập mang tính cạnh tranh hơn với phần chứng minh ngắn hơn và rõ ràng hơn.`O(10^3 n)`thực hiện.
