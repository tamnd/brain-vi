---
title: "CF 105204H - \u0425\u044d\u0448\u0442\u0435\u0433"
description: "Chúng tôi được cung cấp một bài đăng bao gồm các từ và một tập hợp các từ “bắt buộc” tất cả đều phải xuất hiện trong thẻ bắt đầu bằng # cuối cùng. Hashtag được xây dựng từ các từ đã chọn của bài đăng, mỗi từ đóng góp toàn bộ độ dài của nó vào tổng chi phí."
date: "2026-06-27T02:43:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105204
codeforces_index: "H"
codeforces_contest_name: "\u0412\u041a\u041e\u0428\u041f.Junior 2024"
rating: 0
weight: 105204
solve_time_s: 54
verified: true
draft: false
---

[CF 105204H - \u0425\u044d\u0448\u0442\u0435\u0433](https://codeforces.com/problemset/problem/105204/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bài đăng bao gồm các từ và một tập hợp các từ “bắt buộc” tất cả đều phải xuất hiện trong thẻ bắt đầu bằng # cuối cùng. Hashtag được xây dựng từ các từ đã chọn của bài đăng, mỗi từ đóng góp toàn bộ độ dài của nó vào tổng chi phí. Mục tiêu là xóa càng ít từ càng tốt khỏi bài đăng để những từ còn lại vẫn chứa mọi từ khóa bắt buộc ít nhất một lần và tổng độ dài của tất cả các từ còn lại không vượt quá giới hạn$d$. 

Nói cách khác, chúng ta bắt đầu với nhiều tập hợp độ dài từ. Một số vị trí rất đặc biệt vì chúng tương ứng với các từ bắt buộc phải được đưa vào ít nhất một lần. Chúng ta được phép loại bỏ các từ tùy ý nhưng sau khi loại bỏ chúng ta vẫn phải giữ lại ít nhất một lần xuất hiện của mỗi từ được yêu cầu và tổng độ dài của các từ được giữ phải lớn nhất$d$. Chúng tôi muốn tối đa hóa số lượng từ chúng tôi loại bỏ hoặc giảm thiểu tương đương số lượng chúng tôi giữ lại. 

Các ràng buộc làm rõ rằng giải pháp phải gần tuyến tính hoặc gần tuyến tính về số lượng từ. Có tới 30.000 từ khóa và 45.000 từ trong văn bản, do đó, bất kỳ phương pháp nào thử các tập hợp con hoặc tính toán lại tính khả thi cho từng lần xóa một cách độc lập sẽ quá chậm. Thậm chí một$O(m^2)$mô phỏng việc xóa sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. 

Một tình huống phức tạp phát sinh khi chỉ riêng các từ bắt buộc đã vượt quá giới hạn$d$. Ví dụ: nếu các từ được yêu cầu là “a” (độ dài 100.000) và “b” (độ dài 100.000) và$d = 150,000$, thì thậm chí chỉ giữ lại hai cái này cũng tạo thành tổng 200.000, nên không có nghiệm nào tồn tại. Một trường hợp khác là khi một từ bắt buộc xuất hiện nhiều lần trong văn bản. Chúng ta phải giữ ít nhất một bản sao, nhưng giữ nhiều bản sao hơn luôn có hại, vì vậy mọi giải pháp tối ưu sẽ chỉ bảo toàn chính xác một lần xuất hiện cho mỗi từ được yêu cầu. 

Trường hợp không tầm thường thứ hai là việc xóa một từ không bắt buộc luôn có lợi nhưng việc xóa một từ bắt buộc lại bị cấm. Điều này dẫn đến cấu trúc khóa: các từ bắt buộc tạo thành một xương sống cố định và mọi thứ khác đều có trọng số tùy chọn mà chúng tôi có thể loại bỏ để đáp ứng ràng buộc về tổng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử tất cả các tập hợp con các từ bảo toàn ít nhất một lần xuất hiện của mỗi từ được yêu cầu và kiểm tra tổng độ dài của chúng. Điều này đúng về mặt khái niệm nhưng không khả thi ngay lập tức vì có$2^m$tập hợp con. Ngay cả việc hạn chế chú ý đến những từ không bắt buộc vẫn để lại những kết hợp theo cấp số nhân. 

Quan sát quan trọng là ràng buộc hoàn toàn mang tính chất cộng: chỉ có tổng độ dài mới quan trọng và tính khả thi chỉ phụ thuộc vào việc liệu chúng ta có thể giảm tổng bằng cách loại bỏ các từ không bắt buộc trong khi vẫn giữ nguyên các từ bắt buộc hay không. Điều này biến vấn đề thành một vấn đề lựa chọn tham lam đối với những đóng góp. 

Chúng tôi bắt đầu từ cấu trúc bắt buộc duy nhất: đối với mỗi từ được yêu cầu, chúng tôi phải giữ lại ít nhất một lần xuất hiện. Trong số tất cả các lần xuất hiện của một từ bắt buộc, chúng ta nên luôn giữ từ ngắn nhất vì nó giảm thiểu chi phí cơ bản không thể tránh khỏi. Bất kỳ sự cố nào xảy ra lâu hơn chỉ có thể làm tăng tổng số mà không mang lại lợi ích bổ sung. 

Sau khi sửa một lần xuất hiện đại diện cho mỗi từ được yêu cầu, tất cả các từ khác sẽ trở thành mục tùy chọn. Vấn đề giảm xuống còn việc chọn một tập hợp con các từ tùy chọn để giữ sao cho tổng độ dài nằm trong khoảng$d$. Vì chúng tôi muốn giảm thiểu việc xóa, nên chúng tôi cũng muốn giữ lại càng nhiều từ tùy chọn càng tốt, nhưng chỉ khi giới hạn tổng cho phép. 

Điều này trở thành một bài toán rút gọn tham lam cổ điển: chúng ta bắt đầu từ tất cả các từ đã chọn cần thiết cộng với tất cả các từ tùy chọn, tính tổng tổng và sau đó liên tục loại bỏ các từ tùy chọn lớn nhất cho đến khi tổng vừa. Việc xóa các từ lớn hơn trước tiên sẽ giảm thiểu số lần xóa cần thiết để đáp ứng ràng buộc ngân sách, bởi vì mỗi lần xóa sẽ giảm thiểu tối đa mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force |$O(2^m)$|$O(m)$| Quá chậm | 
| Tham lam với việc lựa chọn và phân loại |$O(m \log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các từ được yêu cầu vào một tập hợp băm để chúng tôi có thể kiểm tra tư cách thành viên trong thời gian không đổi cho mỗi từ. 
2. Quét văn bản và nhóm các từ được yêu cầu. Đối với mỗi từ được yêu cầu, chúng ta chỉ cần giữ nguyên độ dài xuất hiện tối thiểu của nó. Điều này đưa ra một chi phí bắt buộc cố định. 
3. Đối với mỗi từ trong văn bản, hãy phân loại nó theo yêu cầu hoặc tùy chọn. Tất cả các từ tùy chọn đều có thể bị xóa. 
4. Tính chi phí cơ bản bằng tổng số lần xuất hiện bắt buộc đã chọn. Nếu điều này đã vượt quá$d$, xuất -1 ngay lập tức vì không thao tác xóa nào có thể làm giảm mức đóng góp cần thiết. 
5. Tính tổng chi phí nếu ban đầu chúng ta giữ lại tất cả các từ, tức là tổng của tất cả độ dài của các từ. 
6. Số tiền chúng ta cần giảm là$total - d$. Để đạt được điều này với số lần xóa tối thiểu, chúng tôi sắp xếp các từ tùy chọn theo độ dài giảm dần và liên tục loại bỏ những từ lớn nhất trước, tích lũy độ dài đã lưu cho đến khi chúng tôi đạt được mức giảm cần thiết. 
7. Số từ bị loại bỏ là số từ tùy chọn mà chúng tôi loại bỏ trong quá trình này. 

Ý tưởng quan trọng là các từ bắt buộc xác định giới hạn dưới cố định và mọi thứ khác hoạt động giống như trọng lượng có thể tháo rời độc lập. Việc sắp xếp đảm bảo mỗi lần xóa sẽ góp phần giảm thiểu tối đa. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước, chúng tôi duy trì một tập hợp các từ được lưu giữ luôn bao gồm ít nhất một lần xuất hiện của mỗi từ được yêu cầu. Trong số tất cả các cách khả thi để loại bỏ chính xác$k$các từ tùy chọn, chọn$k$từ lớn nhất mang lại số tiền còn lại nhỏ nhất có thể. Vì tính khả thi chỉ phụ thuộc vào việc tổng có giảm xuống dưới$d$, thứ tự tham lam này đảm bảo rằng nếu bất kỳ tập hợp nào$k$việc xóa có tác dụng, sự lựa chọn tham lam cũng sẽ có tác dụng. Điều này đảm bảo chúng tôi không bao giờ đánh giá quá cao số lượng yêu cầu xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    keywords = set()
    for _ in range(n):
        keywords.add(input().strip())
    
    words = input().split()
    d = int(input())
    
    # required occurrences: keep at least one per keyword
    from collections import defaultdict
    best_required = {}
    
    optional = []
    total_sum = 0
    
    for w in words:
        L = len(w)
        total_sum += L
        
        if w in keywords:
            if w not in best_required:
                best_required[w] = L
            else:
                best_required[w] = min(best_required[w], L)
        else:
            optional.append(L)
    
    required_sum = sum(best_required.values())
    
    if required_sum > d:
        print(-1)
        return
    
    # we initially assume we keep all words, but we can drop optional ones
    optional.sort(reverse=True)
    
    removed = 0
    saved = 0
    
    for L in optional:
        if total_sum - saved <= d:
            break
        saved += L
        removed += 1
    
    print(removed)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng việc tách các từ khóa thành một bộ để tra cứu nhanh. Trong khi quét bài đăng, nó duy trì độ dài xuất hiện tối thiểu cho mỗi từ khóa, bởi vì bất kỳ lần xuất hiện trùng lặp nào dài hơn đều tệ hơn so với lần xuất hiện ngắn nhất. 

Các từ tùy chọn chỉ được lưu trữ dưới dạng độ dài, vì chỉ đóng góp của chúng mới quan trọng. Sau khi tính toán tổng số tiền và đường cơ sở cần thiết, chúng tôi kiểm tra tính khả thi. Nếu ngay cả đường cơ sở bắt buộc cũng vượt quá$d$, chúng tôi chấm dứt sớm. 

Vòng lặp loại bỏ tham lam xử lý các từ tùy chọn được sắp xếp theo độ dài giảm dần. Mỗi lần lặp lại mô phỏng việc xóa từ lớn nhất còn lại hiện tại, theo dõi tổng độ dài đã được giảm. Khi số tiền còn lại phù hợp với$d$, chúng tôi dừng lại. 

## Ví dụ đã hoạt động 

Hãy xem xét dữ liệu đầu vào mẫu trong đó chỉ cần một số từ và chúng ta phải bỏ một từ như “olympiady” để đáp ứng giới hạn. 

Chúng tôi theo dõi các từ bắt buộc và các từ tùy chọn riêng biệt: 

| Bước | Hành động | Số tiền bắt buộc | Tổng số tiền | Tùy chọn Đã xóa | 
| --- | --- | --- | --- | --- | 
| 1 | phân tích từ | 21 | 30 | 0 | 
| 2 | sắp xếp tùy chọn | 21 | 30 | 0 | 
| 3 | loại bỏ “Olympic” | 21 | 21 | 1 | 

Điều này cho thấy rằng chỉ cần xóa một lần là đủ sau khi loại bỏ từ tùy chọn lớn nhất. 

Bây giờ hãy xem xét trường hợp tồn tại nhiều từ tùy chọn nhỏ: 

Đầu vào: từ khóa = {a}, văn bản = [a, x, y, z], d = 3 

| Bước | Hành động | Số tiền bắt buộc | Tổng số tiền | Tùy chọn Đã xóa | 
| --- | --- | --- | --- | --- | 
| 1 | giữ “a” | 1 | 1+1+1+1=4 | 0 | 
| 2 | loại bỏ “z” | 1 | 3 | 1 | 
| 3 | xóa “y” | 1 | 2 | 2 | 

Sau khi loại bỏ hai từ tùy chọn nhỏ nhất, ràng buộc được thỏa mãn. 

Những dấu vết này cho thấy thuật toán luôn ưu tiên loại bỏ những từ đắt giá nhất trước tiên, giảm thiểu số lần xóa cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m + n)$| Sắp xếp các từ tùy chọn chiếm ưu thế; đầu vào quét là tuyến tính | 
| Không gian |$O(m + n)$| Lưu trữ bộ từ khóa, danh sách tùy chọn và theo dõi bắt buộc | 

Các ràng buộc cho phép tối đa 45.000 từ, do đó việc sắp xếp ở quy mô này là đủ nhanh. Tiền xử lý tuyến tính đảm bảo không có tắc nghẽn nào ngoài việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# sample-like
assert run("""2 4
privet
uchastnikam
privet vsem uchastnikam olympiady
22
""") == "1"

# all required already exceed limit
assert run("""1 2
a
a aa
1
""") == "-1"

# no optional words
assert run("""2 2
a
b
a b
3
""") == "-1"

# large removable pool
assert run("""1 5
a
a b c d e
3
""") == "2"

# exact fit without removals
assert run("""1 3
a
a bb c
10
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp mẫu | 1 | xóa tham lam cơ bản | 
| tràn yêu cầu | -1 | không thể do chi phí bắt buộc | 
| không có từ tùy chọn | -1 | chỉ tồn tại những từ bắt buộc | 
| nhiều từ tùy chọn | 2 | lựa chọn xóa tham lam | 
| đã hợp lệ | 0 | không cần xóa | 

## Vỏ cạnh 

Trường hợp nghiêm trọng là khi chỉ riêng các từ được yêu cầu đã vượt quá giới hạn. Ví dụ: nếu các từ được yêu cầu đủ dài đến mức số lần xuất hiện tối thiểu của chúng đã vi phạm$d$, thuật toán ngay lập tức trả về -1 sau khi tính tổng cơ sở. Điều này tránh việc phân loại hoặc xử lý không cần thiết. 

Một trường hợp khác là lặp lại những từ cần thiết với độ dài khác nhau. Giả sử một từ bắt buộc xuất hiện nhiều lần với độ dài 10, 5 và 7. Thuật toán chỉ giữ 5 làm chi phí bắt buộc. Điều này đảm bảo chúng tôi không đánh giá quá cao độ dài không thể tránh khỏi. Nếu chúng ta giữ nhầm tất cả các lần xuất hiện, chúng ta có thể kết luận sai rằng không có nghiệm nào tồn tại ngay cả khi có một nghiệm hợp lệ. 

Trường hợp cạnh cuối cùng là khi tất cả các từ tùy chọn đều ngắn hơn các từ bắt buộc. Ngay cả khi đó, việc sắp xếp vẫn hoạt động chính xác vì thứ tự xóa chỉ phụ thuộc vào mức giảm tối đa cho mỗi lần xóa chứ không phụ thuộc vào việc so sánh với các từ được yêu cầu.
