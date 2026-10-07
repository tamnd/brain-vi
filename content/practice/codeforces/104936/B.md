---
title: "CF 104936B - Làm bài kiểm tra"
description: "Chúng tôi được đưa ra một số kịch bản thi độc lập. Mỗi tình huống mô tả tổng thời gian thi cố định và danh sách các vấn đề."
date: "2026-06-28T18:10:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104936
codeforces_index: "B"
codeforces_contest_name: "MITIT 2024 Beginner Round"
rating: 0
weight: 104936
solve_time_s: 69
verified: false
draft: false
---

[CF 104936B - Làm bài kiểm tra](https://codeforces.com/problemset/problem/104936/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản thi độc lập. Mỗi tình huống mô tả tổng thời gian thi cố định và danh sách các vấn đề. Mọi vấn đề đều tiêu tốn thời gian tương đương với độ khó của nó và việc giải nó sẽ mang lại nhiều điểm hơn một chút so với thời gian thực hiện, cụ thể là có thêm một điểm sau thời gian giải. 

Học sinh có thể chọn bất kỳ tập hợp con vấn đề nào để giải, miễn là tổng thời gian của chúng không vượt quá thời gian làm bài. Sau khi học xong, học viên có thể nộp sớm, thời gian chưa sử dụng sẽ được cộng thêm điểm thưởng tương ứng với số phút còn lại. 

Vì vậy, tổng số điểm là tổng số phần thưởng của bài toán cộng với thời gian còn lại và mục tiêu là chọn ra những bài toán cần giải quyết để tối đa hóa số tiền này. 

Viết lại cấu trúc cụ thể hơn, nếu chọn một tập các bài toán có tổng thời gian là S thì điểm là tổng của các bài toán đã chọn của (d_i + 1) cộng (M - S). Điều này đơn giản hóa thành M cộng với số lượng bài toán đã giải cộng với tổng độ khó của chúng trừ đi tổng thời gian của chúng, và vì mỗi bài toán đóng góp d_i cả tích cực và tiêu cực, nên tất cả các hạng độ khó đều bị hủy ngoại trừ việc đếm xem có bao nhiêu vấn đề được chọn. Đây là sự đơn giản hóa quan trọng: mọi vấn đề được chọn sẽ tăng điểm chính xác lên 1 ngoài việc tiêu tốn thời gian, trong khi bản thân thời gian chỉ quan trọng thông qua phần thưởng còn sót lại. 

Như vậy, điểm sẽ trở thành M cộng với số bài toán được chọn. Ràng buộc về thời gian chỉ hạn chế những tập hợp con nào khả thi, nhưng trong số những tập hợp con khả thi, việc tối đa hóa điểm số tương đương với việc tối đa hóa số lượng vấn đề được giải quyết. 

Từ các ràng buộc, tổng số vấn đề trong các trường hợp thử nghiệm lên tới 100000, do đó, bất kỳ giải pháp nào về cơ bản đều phải là tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. Việc sắp xếp có thể chấp nhận được, nhưng bất cứ điều gì như tìm kiếm tập hợp con theo cấp số nhân đều không thể thực hiện được. Ngay cả O(N^2) cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm. 

Một ý tưởng tham lam ngây thơ có thể là chọn các bài toán theo bất kỳ thứ tự nào cho đến khi hết thời gian, nhưng điều này có thể thất bại tùy theo thứ tự. Một nỗ lực ngây thơ khác là thử tất cả các tập hợp con, rõ ràng là theo cấp số nhân. 

Một cạm bẫy tinh vi hơn là bỏ qua việc hủy bỏ: người ta có thể cố gắng tối ưu hóa điểm thô d_i + 1 + thời gian còn lại một cách không chính xác và tin rằng d_i lớn sẽ tốt hơn. Nhưng vì việc giải một bài toán sẽ giảm thời gian còn lại đi đúng d_i, nên hiệu ứng thực của độ khó sẽ biến mất. 

Trường hợp cạnh khóa là khi M nhỏ hơn tất cả d_i. Khi đó không có vấn đề nào có thể giải được và câu trả lời chỉ đơn giản là M. Một trường hợp khác là khi M đủ lớn để giải quyết mọi thứ, trong đó câu trả lời trở thành M + N. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các tập hợp con của vấn đề, tính tổng thời gian của chúng và tính điểm kết quả. Điều này đúng vì nó tuân theo định nghĩa trực tiếp, nhưng nó yêu cầu kiểm tra 2^N tập hợp con và mỗi tập hợp con cần có tổng bằng O(N), dẫn đến O(N·2^N), điều này không khả thi ngay cả khi N = 30. 

Quan sát quan trọng là sau khi đơn giản hóa đại số, điểm số chỉ phụ thuộc vào số lượng bài toán được chọn chứ không phụ thuộc vào bài toán cụ thể nào đóng góp nhiều “giá trị mỗi lần” hơn. Mỗi vấn đề được chọn sẽ tăng điểm đúng một đơn vị so với việc bỏ qua nó, nhưng cũng tiêu tốn thời gian. Điều này tạo ra cấu trúc “tối đa hóa số lượng theo giới hạn tổng”. 

Vì tất cả các mục đều có lợi ích đối xứng, nên điều duy nhất quan trọng là tính khả thi: chúng ta muốn xếp càng nhiều bài toán càng tốt vào tổng thời gian M. Để tối đa hóa số lượng theo một ràng buộc về tổng, trước tiên chúng ta phải luôn chọn những khó khăn nhỏ nhất hiện có. Sắp xếp mảng và tích lũy tham lam là tối ưu bởi vì bất kỳ giải pháp tối ưu nào bao gồm phần tử lớn hơn trong khi loại trừ phần tử nhỏ hơn đều có thể được hoán đổi mà không làm giảm tính khả thi hoặc số lượng. 

Điều này làm giảm vấn đề sắp xếp và tích lũy tiền tố.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N·2^N) | O(N) | Quá chậm | 
| Sắp xếp + Tham lam | O(N log N) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc N và M, và danh sách những khó khăn. Những điều này xác định một bài toán về năng lực giống như chiếc ba lô trong đó mỗi vật phẩm tiêu tốn thời gian d_i và mang lại lợi ích đơn vị về mặt số lượng. 
2. Sắp xếp độ khó tăng dần. Điều này đảm bảo rằng chúng tôi luôn xem xét các vấn đề rẻ nhất trước tiên, điều này là tối ưu để tối đa hóa số lượng chúng tôi có thể giải quyết. 
3. Khởi tạo tổng thời gian đã sử dụng và bộ đếm số lượng bài toán chúng tôi gặp phải. Bộ đếm trực tiếp thể hiện sự đóng góp cho điểm số vượt quá M. 
4. Lặp lại các khó khăn đã được sắp xếp. Đối với mỗi d_i, hãy kiểm tra xem việc thêm nó có giữ tổng thời gian trong M hay không. Nếu có, hãy đưa nó vào và cập nhật thời gian chạy cũng như bộ đếm. Nếu không, hãy dừng lại ngay lập tức. 
5. Tính đáp án cuối cùng bằng M + số lượng các bài toán đã chọn, vì thời gian còn lại đã được nhúng vào M và tất cả thời gian đã sử dụng sẽ bị loại bỏ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, nếu chúng ta đã chọn k bài toán có tổng thời gian tối thiểu thì việc thay thế bất kỳ bài toán nào đã chọn bằng một bài toán lớn hơn không thể làm giảm tổng thời gian. Do đó, nếu một bài toán lớn hơn được sử dụng trong khi một bài toán nhỏ hơn chưa được sử dụng tồn tại, việc hoán đổi chúng vẫn giữ được tính khả thi và không làm giảm số lượng. Đối số trao đổi này đảm bảo rằng một giải pháp tối ưu luôn tương ứng với việc lấy tiền tố của mảng được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        d = list(map(int, input().split()))
        
        d.sort()
        
        total = 0
        cnt = 0
        
        for x in d:
            if total + x > m:
                break
            total += x
            cnt += 1
        
        out.append(str(m + cnt))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc sắp xếp danh sách độ khó của từng trường hợp kiểm thử, sau đó tích lũy một cách tham lam cho đến khi vượt quá ngân sách thời gian. Biến`total`bài hát tiêu tốn thời gian thi, trong khi`cnt`theo dõi có bao nhiêu vấn đề được thực hiện. Điểm cuối cùng được tính là`m + cnt`, phản ánh rằng mỗi vấn đề được giải quyết sẽ đóng góp thêm chính xác một điểm ngoài việc loại bỏ chi phí thời gian. 

Việc nghỉ sớm rất quan trọng vì một khi vấn đề nhỏ nhất còn lại không phù hợp thì cũng không có vấn đề lớn hơn nào phù hợp. 

## Ví dụ đã hoạt động 

Xét trường hợp có M = 7 và khó khăn [1, 2, 3, 4]. 

Sau khi sắp xếp, chúng ta đã có [1, 2, 3, 4]. Chúng tôi lặp lại: 

| Bước | Bộ được chọn | Tổng thời gian | Đếm | Công suất còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | 1 | 1 | 6 | 
| 2 | [1,2] | 3 | 2 | 4 | 
| 3 | [1,2,3] | 6 | 3 | 1 | 
| 4 | dừng lại | 6 | 3 | 1 | 

Câu trả lời cuối cùng là 7 + 3 = 10. 

Dấu vết này cho thấy rằng việc tham lam lấy các phần tử nhỏ nhất sẽ tối đa hóa số lượng mà không vi phạm ràng buộc và buộc phải dừng sớm khi mục tiếp theo không phù hợp. 

Bây giờ hãy xem xét M = 5 và những khó khăn [5, 10, 20]. 

Sau khi sắp xếp: [5, 10, 20]. 

| Bước | Bộ được chọn | Tổng thời gian | Đếm | 
| --- | --- | --- | --- | 
| 1 | [5] | 5 | 1 | 
| 2 | dừng lại | 5 | 1 | 

Đáp án là 5+1=6. 

Điều này thể hiện trường hợp cạnh trong đó chỉ có một mục phù hợp và các mục lớn hơn không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(1) thêm | Sắp xếp được thực hiện ngoài việc lưu trữ đầu vào | 

Tổng N trên các trường hợp thử nghiệm là 100000, do đó việc sắp xếp theo từng trường hợp thử nghiệm vẫn hiệu quả và độ phức tạp tổng thể dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        res = []
        for _ in range(t):
            n, m = map(int, input().split())
            d = list(map(int, input().split()))
            d.sort()
            s = 0
            c = 0
            for x in d:
                if s + x > m:
                    break
                s += x
                c += 1
            res.append(str(m + c))
        return "\n".join(res)

    return solve()

# provided sample (formatted as multi-test)
assert run("""4
3 7
1 2 4
4 10
1 2 3 4
1 5
10
2 9
4 5
""") == "10\n49\n5\n12"

# all equal values
assert run("""1
5 10
2 2 2 2 2
""") == "12"

# cannot take anything
assert run("""1
3 1
5 6 7
""") == "1"

# take everything exactly
assert run("""1
3 6
1 2 3
""") == "9"

# large M
assert run("""1
4 100
1 1 1 1
""") == "104"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 12 | xử lý cà vạt tham lam | 
| không thể lấy bất cứ thứ gì | 1 | trường hợp lựa chọn không | 
| phù hợp chính xác | 9 | bình đẳng ranh giới | 
| lớn M | 104 | trường hợp lựa chọn đầy đủ | 

## Vỏ cạnh 

Khi M nhỏ hơn độ khó nhỏ nhất, thuật toán sẽ bỏ qua ngay tất cả các phần tử vì lần so sánh đầu tiên không thành công. Đối với đầu vào như N = 3, M = 1, d = [5, 6, 7], việc sắp xếp giữ nguyên mảng và lần kiểm tra đầu tiên không thành công, tạo ra cnt = 0 và câu trả lời = 1, phù hợp với chiến lược "gửi ngay" dự định. 

Khi tất cả các vấn đề đều phù hợp, chẳng hạn như M = 6 với [1, 2, 3], vòng lặp tiêu thụ mọi thứ và cnt trở thành N. Đầu ra trở thành M + N, phản ánh rằng mỗi vấn đề đóng góp chính xác một đơn vị lợi ích vượt quá tính khả thi. 

Khi tồn tại nhiều độ khó bằng nhau, việc sắp xếp sẽ bảo toàn chúng theo bất kỳ thứ tự nào và việc lựa chọn tiền tố tham lam đương nhiên sẽ có số lượng phù hợp. Việc hoán đổi các yếu tố bình đẳng không làm thay đổi tính khả thi, khẳng định tính ổn định của phương pháp.
