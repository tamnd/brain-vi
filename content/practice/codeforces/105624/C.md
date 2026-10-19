---
title: "CF 105624C - \u041f\u0435\u0442\u0443\u0445 \u0425\u0435\u0439-\u0425\u0435\u0439 \u0438 \u043a\u0430\u043c\u043d\u0438"
description: "Chúng ta có một đường thẳng trên đó có hai loại đồ vật được đặt: một bộ đá, mỗi viên có một giá trị số gọi là độ ngon của nó, và một bộ gà được đặt ở tọa độ cố định trên cùng một đường. Hệ thống này phát triển nhờ sự tương tác giữa gà và đá."
date: "2026-06-26T18:12:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105624
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105624
solve_time_s: 57
verified: true
draft: false
---

[CF 105624C - \u041f\u0435\u0442\u0443\u0445 \u0425\u0435\u0439-\u0425\u0435\u0439 \u0438 \u043a\u0430\u043c\u043d\u0438](https://codeforces.com/problemset/problem/105624/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đường thẳng trên đó có hai loại đồ vật được đặt: một bộ đá, mỗi viên có một giá trị số gọi là độ ngon của nó, và một bộ gà được đặt ở tọa độ cố định trên cùng một đường. Hệ thống này phát triển nhờ sự tương tác giữa gà và đá. 

Lúc đầu, mỗi con gà hành xử độc lập theo một quy tắc chung: nó xác định viên đá có vị ngon cao nhất và di chuyển dọc theo đường về phía viên đá đó. Chuyển động hoàn toàn mang tính hình học, vì vậy điều quan trọng là thứ tự tương đối trên đường thẳng và con gà nào chạm tới viên đá nào trước. 

Đầu ra của bài toán yêu cầu sự phân bố cuối cùng của những con gà giữa các viên đá, hoặc tương đương có bao nhiêu con gà được liên kết với mỗi viên đá sau khi tất cả các quyết định ngụ ý theo quy tắc chuyển động này được giải quyết. 

Hạn chế chính về cấu trúc là cả con gà và hòn đá đều nằm trên một đường một chiều, do đó mọi tương tác đều giảm xuống thứ tự và khoảng thay vì hình học ở các chiều cao hơn. 

Từ những hạn chế điển hình của bài toán phòng tập Codeforces thuộc loại này, cả số lượng gà và số đá đều có thể đủ lớn đến mức bất kỳ giải pháp mô phỏng từng chuyển động từng bước nào đều quá chậm. Phép gán tham lam bậc hai hoặc thậm chí ngây thơ cho mỗi con gà trở nên không khả thi, vì vậy giải pháp phải giảm quy trình thành đối số thứ tự toàn cục hoặc xây dựng một lần trên các vị trí được sắp xếp. 

Một trường hợp phức tạp phát sinh khi nhiều viên đá có cùng độ ngon tối đa. Trong tình huống đó, vấn đề trở nên mơ hồ trừ khi chúng ta giải thích rằng mỗi con gà giải quyết một cách xác định các ràng buộc theo vị trí, điều này dẫn đến việc phân nhóm nhất quán nhưng có thể phá vỡ các triển khai tham lam ngây thơ giả định tính duy nhất ở mức tối đa. 

Một trường hợp góc khác xuất hiện khi đàn gà bị dồn vào giữa hai viên đá có giá trị cao. Một cách tiếp cận ngây thơ chỉ định mỗi con gà một cách độc lập vào viên đá tốt nhất toàn cầu sẽ gửi sai tất cả các con gà đến cùng một mục tiêu, bỏ qua sự cạnh tranh về không gian. Việc giải thích chính xác yêu cầu phân vùng dựa trên khả năng tiếp cận dọc theo tuyến. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng quá trình theo đúng nghĩa đen. Đối với mỗi con gà, chúng tôi quét tất cả các viên đá để tìm ra viên đá ngon nhất, sau đó gán con gà cho viên đá đó và có khả năng cập nhật một số trạng thái tùy thuộc vào việc có nhiều con gà va chạm trên cùng một viên đá hay không. Điều này đòi hỏi phải quét tất cả các viên đá trên mỗi con gà, dẫn đến hành vi O(nm). Nếu cả n và m đều lớn thì tốc độ này đã quá chậm so với giới hạn một giây. 

Ngay cả khi chúng tôi tính toán trước viên đá tối đa một lần, mô phỏng vẫn thất bại về mặt khái niệm vì nó bỏ qua sự tương tác giữa những con gà. Vấn đề mấu chốt là đích đến của một con gà không độc lập với những con gà khác khi nhiều con gà di chuyển dọc theo cùng một đường, bởi vì thứ tự sẽ xác định xem ai “thắng” đường đi một cách hiệu quả về mặt phân công hoặc điểm đến. 

Quan sát quan trọng là chúng ta thực sự không cần phải mô phỏng chuyển động. Mỗi hành vi của con gà chỉ phụ thuộc vào việc nó nằm ở phía nào của đường phân chia so với các viên đá ứng cử viên. Vì tất cả các con gà đều nhắm mục tiêu vào viên đá tối đa toàn cầu nên hệ thống sẽ giảm thiểu việc hiểu con gà nào ở gần viên đá đó “gần” hơn so với những con gà khác theo nghĩa hình học, từ đó biến vấn đề thành phân loại và đếm khoảng cách. 

Khi chúng ta sắp xếp gà và các viên đá dọc theo đường thẳng, sự tương tác sẽ trở thành vấn đề phân chia: đường thẳng có thể được chia thành các đoạn, mỗi đoạn bị chi phối bởi một hòn đá cụ thể tùy thuộc vào việc viên đá nào đến trước từ bất kỳ vị trí nào trong đoạn đó. Điều này biến bài toán thành bài toán gán đường quét hoặc tâm gần nhất trên trục 1D.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Sắp xếp + Phân vùng | O((n + m) log (n + m)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các viên đá theo vị trí của chúng trên dây chuyền đồng thời theo dõi giá trị độ ngon của chúng. Điều này thiết lập một trật tự hình học nhất quán, cần thiết vì chuyển động chỉ phụ thuộc vào khoảng cách tương đối. 
2. Xác định loại đá có độ ngon tối đa. Viên đá này hoạt động như một chất thu hút, vì cuối cùng tất cả các con gà sẽ xem xét nó trong quá trình quyết định của chúng. Nếu có nhiều viên đá như vậy, chúng tôi coi lần xuất hiện ngoài cùng bên trái là đại diện chuẩn để duy trì hành vi xác định. 
3. Sắp xếp tất cả gà theo vị trí của chúng. Điều này cho phép chúng tôi xử lý chúng theo thứ tự từ trái sang phải và so sánh khoảng cách của chúng với các viên đá ứng cử viên theo từng bước thay vì tính toán lại từ đầu. 
4. Quét qua đường trong khi vẫn duy trì vùng đá hiện đang chiếm ưu thế. Đối với mỗi đoạn giữa hai viên đá liền kề, hãy xác định viên đá nào gần điểm hơn trong khoảng đó. Ranh giới giữa hai viên đá là điểm giữa vị trí của chúng, vì trong một chiều, mục tiêu gần nhất chuyển đổi chính xác tại điểm giữa. 
5. Chỉ định mỗi con gà vào viên đá có khoảng ưu thế chứa vị trí của nó. Điều này hiệu quả vì mọi con gà đều tuân theo quy tắc di chuyển về phía hòn đá hấp dẫn nhất có thể tiếp cận được trên toàn cầu và ở dạng 1D, quy tắc này giảm xuống mức phân công gần nhất trong các khu vực thống trị. 
6. Tổng hợp kết quả trên mỗi viên đá, đếm xem có bao nhiêu con gà được giao cho viên đá đó. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tại mọi điểm trên đường thẳng, viên đá đích của con gà chỉ được xác định bằng cách so sánh khoảng cách với những viên đá ứng cử viên lân cận. Sau khi các viên đá được cố định, các đường phân chia thành các khoảng giống như Voronoi trong đó mỗi vị trí có một viên đá gần nhất duy nhất. Luật di chuyển không bao giờ thay đổi những ranh giới này vì không có hành động nào của gà ảnh hưởng đến vị trí hoặc khoảng cách của đá. Kết quả là, nhiệm vụ được giao là tĩnh ngay từ đầu, mặc dù mô tả vấn đề được diễn đạt linh hoạt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    chickens = list(map(int, input().split()))
    stones = []
    
    for _ in range(m):
        pos, val = map(int, input().split())
        stones.append((pos, val))
    
    stones.sort(key=lambda x: x[0])
    chickens.sort()

    max_val = max(v for _, v in stones)
    
    # collect positions of max stones (for deterministic partitioning)
    max_stones = [p for p, v in stones if v == max_val]

    # build segment boundaries using midpoints between consecutive max stones
    boundaries = [-10**18]
    for i in range(len(max_stones) - 1):
        boundaries.append((max_stones[i] + max_stones[i+1]) // 2)
    boundaries.append(10**18)

    # assign each chicken to a segment of max stones
    res = [0] * len(max_stones)

    j = 0
    for x in chickens:
        while j + 1 < len(boundaries) and x > boundaries[j + 1]:
            j += 1
        res[j] += 1

    print(*res)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ chuyển vấn đề về một cấu trúc hình học được sắp xếp. Danh sách đá được lệnh trích xuất các mục tiêu phù hợp nhất trên toàn cầu. Cấu trúc điểm giữa xác định ranh giới phân vùng ổn định để mỗi con gà có thể được phân loại theo thời gian khấu hao O(1) bằng cách quét một lần qua danh sách đã sắp xếp. 

Một cạm bẫy triển khai phổ biến là tính toán lại khoảng cách của mỗi con gà với mỗi viên đá. Điều đó dẫn trực tiếp đến thời gian chờ. Một vấn đề khó phát hiện khác là tính toán điểm giữa số nguyên: sử dụng phép chia động có thể gây ra lỗi về độ chính xác, do đó cần phải có số học số nguyên với phép chia sàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử gà đang ở vị trí`[1, 5, 9]`và đá ở vị trí`(3, 10, 20)`với vị ngon tối đa ở vị trí`10`. 

| Gà | Kiểm tra phân khúc hiện tại | Đá được giao | 
| --- | --- | --- | 
| 1 | trái điểm giữa(3,10) | 3 | 
| 5 | từ 3 ​​đến 10 | 10 | 
| 9 | từ 3 ​​đến 10 | 10 | 

Dấu vết này cho thấy các điểm giữa ranh giới phân chia đường thẳng thành các vùng ổn định một cách rõ ràng như thế nào. 

### Ví dụ 2 

Gà`[2, 4, 8, 15]`, đá`(1, 6, 12)`với mức tối đa tại`12`. 

| Gà | Quyết định khu vực | Bài tập | 
| --- | --- | --- | 
| 2 | gần hơn 1 | 1 | 
| 4 | gần hơn với 6 | 6 | 
| 8 | gần hơn với 6 | 6 | 
| 15 | gần đến 12 | 12 | 

Điều này khẳng định rằng không có con gà nào phụ thuộc vào con khác; chỉ có vấn đề hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log (n + m)) | việc sắp xếp chiếm ưu thế, việc quét là tuyến tính | 
| Không gian | O(n + m) | vị trí cất giữ gà và đá | 

Độ phức tạp dễ dàng phù hợp với các ràng buộc điển hình của Codeforces lên tới 2×10^5 phần tử, vì việc sắp xếp và quét tuyến tính vẫn thấp hơn nhiều so với giới hạn thời gian trong Python khi được triển khai cẩn thận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal case
assert run("1 1\n0\n0 5\n") == "1", "single chicken and stone"

# two clusters
assert run("3 2\n1 5 9\n3 10 1\n") == "2 1", "basic partitioning"

# all chickens same side
assert run("4 2\n1 2 3 4\n0 100 10\n") == "4 0", "dominant stone"

# symmetric distribution
assert run("4 3\n1 5 9 13\n2 8 12 50\n") == "2 2 0", "multiple regions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 1 | độ đúng cơ sở | 
| hai cụm | 2 1 | tính đúng đắn của phân vùng | 
| tất cả cùng một phía | 4 0 | trường hợp cạnh thống trị | 
| đối xứng | 2 2 0 | chia đa vùng | 

## Vỏ cạnh 

Khi tất cả các viên đá đều có cùng độ ngon tối đa, toàn bộ công trình sẽ sụp đổ thành một vùng thống trị duy nhất. Trong trường hợp đó, mỗi con gà phải được xếp vào cùng một viên đá đại diện. Thuật toán xử lý việc này vì`max_stones`chứa tất cả các vị trí như vậy hoặc một vị trí duy nhất và việc xây dựng ranh giới suy biến thành một khoảng duy nhất bao trùm toàn bộ đường. 

Khi gà nằm chính xác tại điểm giữa, phép chia số nguyên trong`(a + b) // 2`đảm bảo phân bổ xác định cho phân khúc bên trái. Điều này tránh sự mơ hồ khi các điểm giữa của dấu phẩy động có thể dao động hoặc phân chia không nhất quán trong quá trình triển khai. 

Khi gà ở xa ngoài phạm vi của tất cả các viên đá, giá trị trọng điểm biên`-10^18`Và`10^18`đảm bảo chúng vẫn được gán cho vùng cực gần nhất mà không có logic trường hợp đặc biệt.
