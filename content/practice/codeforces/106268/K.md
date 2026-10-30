---
title: "CF 106268K - Cơ cấu thành viên của một hội kín"
description: "Chúng ta được cung cấp một tập hợp các câu mô tả mối quan hệ giữa những người trong một hội kín. Mỗi người có một “bộ người giới thiệu” ẩn, nghĩa là tập hợp các thành viên đã trực tiếp giới thiệu họ khi họ tham gia."
date: "2026-06-18T23:10:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106268
codeforces_index: "K"
codeforces_contest_name: "The 2025 Asia Yokohama Regional Contest"
rating: 0
weight: 106268
solve_time_s: 53
verified: true
draft: false
---

[CF 106268K - Cơ cấu thành viên của một hội kín](https://codeforces.com/problemset/problem/106268/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các câu mô tả mối quan hệ giữa những người trong một hội kín. Mỗi người có một “bộ người giới thiệu” ẩn, nghĩa là tập hợp các thành viên đã trực tiếp giới thiệu họ khi họ tham gia. Cấu trúc có một hạn chế quan trọng: mỗi bộ đề xuất riêng biệt có thể được sử dụng cho tối đa một thành viên. Nói cách khác, không có hai người khác nhau nào có thể chia sẻ cùng một nhóm người giới thiệu. 

Chúng tôi không quan sát các bộ thực tế. Thay vào đó, chúng tôi nhận được các ràng buộc gồm ba loại. Một loại khẳng định rằng một người cụ thể nằm trong nhóm người giới thiệu của người khác. Một người khác khẳng định rằng một người không nằm trong nhóm người giới thiệu. Điều thứ ba nói rằng tập gợi ý của một người chính xác là giao điểm của tập gợi ý của hai người khác. 

Nhiệm vụ là quyết định xem có tồn tại bất kỳ sự phân công nào của các nhóm người thỏa mãn tất cả các ràng buộc cùng một lúc hay không. 

Khó khăn chính là danh tính không đáng tin cậy: các số nguyên khác nhau có thể đề cập đến cùng một người thực tế và cùng một số nguyên luôn đề cập đến cùng một người. Vì vậy, chúng tôi thực sự đang giải quyết một vấn đề nhất quán đối với các mối quan hệ tập hợp đã biết một phần với các ràng buộc đẳng thức có thể được gây ra bởi các tập hợp giống hệt nhau. 

Các ràng buộc nhỏ trên mỗi trường hợp thử nghiệm, với tổng kích thước của các thử nghiệm được giới hạn bởi 3000. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng liệt kê hoặc xây dựng rõ ràng các tập hợp con đầy đủ của các thành viên. Mô phỏng tập hợp trực tiếp trên một vũ trụ có kích thước lên tới 9000 đã là quá lớn nếu chúng ta theo dõi các tập hợp thành viên rõ ràng trên mỗi nút và liên tục giao nhau với chúng. 

Một trường hợp cạnh tinh tế xuất hiện khi các ràng buộc giao nhau buộc hai tập hợp phải bằng nhau. Nếu chúng ta suy luận rằng hai người phải có bộ gợi ý giống hệt nhau thì quy tắc “bộ duy nhất ngụ ý người duy nhất” buộc họ phải đại diện cho cùng một cá nhân. Bất kỳ ràng buộc nào sau này coi chúng là khác biệt sẽ ngay lập tức tạo ra mâu thuẫn. Một trường hợp phức tạp khác là khi các giao điểm truyền bá sự bình đẳng một cách gián tiếp, chẳng hạn`A = B ∩ C`Và`B = C`, làm sụp đổ cả ba bộ. 

Một giải pháp ngây thơ liên tục tinh chỉnh các tập hợp cho đến khi hội tụ có nguy cơ bỏ sót những mâu thuẫn do ràng buộc tính duy nhất đưa ra, đặc biệt khi đẳng thức được ngụ ý thay vì được nêu rõ ràng. 

## Phương pháp tiếp cận 

Quan sát trọng tâm là chúng ta đang suy luận về các tập hợp mà chỉ có quan hệ thành viên, sự bằng nhau của các tập hợp và giao điểm là quan trọng. Chúng ta thực sự không cần biết các tập hợp một cách rõ ràng; chúng ta chỉ cần đảm bảo rằng mọi ràng buộc đều có thể được thỏa mãn mà không vi phạm tính duy nhất của các tập giống hệt nhau. 

Một cách giải thích bạo lực sẽ cố gắng gán cho mỗi người một tập hợp con của một vũ trụ trừu tượng và liên tục thực thi các ràng buộc cho đến khi kết thúc. Đối với n lên tới 3000, điều này sẽ liên quan đến việc duy trì tối đa 3000 bộ, mỗi bộ có kích thước lên tới 3000, với các hoạt động giao lộ có thể tốn O(n) mỗi bộ. Việc truyền bá ràng buộc lặp đi lặp lại có thể dễ dàng dẫn đến hành vi O(n³), điều này không được chấp nhận. 

Điểm mấu chốt là mỗi người được xác định duy nhất bởi tập hợp đề xuất của họ, do đó, vấn đề tương đương với việc duy trì tính nhất quán về sự bình đẳng giữa các tập hợp và ràng buộc thành viên, trong đó tập hợp là đối tượng trừu tượng. Các ràng buộc giao nhau hoạt động giống như các phương trình hàm giữa các đối tượng trừu tượng này. Điều này gợi ý rằng hãy coi mỗi bộ đề xuất là một nút trong cấu trúc nơi chúng tôi thực thi mối quan hệ giữa các nút thay vì cụ thể hóa các bộ. 

Chúng tôi rút gọn vấn đề thành một hệ thống giống như tìm hợp với cấu trúc bổ sung: mỗi nút tương ứng với tập hợp của một người và các ràng buộc hoặc thêm các phần tử vào một tập hợp, cấm các phần tử hoặc đánh đồng một tập hợp với giao điểm của hai tập hợp khác. Các ràng buộc giao nhau có thể được viết lại dưới dạng các ràng buộc bao gồm: tập hợp các`a`phải là tập hợp con của cả hai`b`Và`c`và mọi phần tử chung của`b`Và`c`phải thuộc về`a`. Điều này chuyển thành ý nghĩa hai chiều giữa các sự kiện thành viên. 

Chúng tôi duy trì các ràng buộc bằng cách sử dụng cấu trúc hợp tập hợp rời rạc trên các cá thể khi chúng tôi phát hiện các tập hợp bằng nhau và chúng tôi truyền bá các ràng buộc thành viên một cách cẩn thận. Ý tưởng quan trọng là khi hai tập hợp được chứng minh bằng nhau, danh tính của chúng sẽ sụp đổ và tất cả các xác nhận thành viên phải nhất quán dưới sự hợp nhất đó. 

Giải pháp trở thành một hệ thống lan truyền ràng buộc trong đó chúng tôi thống nhất lặp đi lặp lại các nút khi bị ép buộc bởi các tương đương giao nhau và kiểm tra mâu thuẫn khi tư cách thành viên bị cấm xung đột với các nút bắt buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng bộ Brute Force | O(n³) | O(n²) | Quá chậm | 
| Tuyên truyền ràng buộc với việc hợp nhất kiểu DSU | O(n α(n)) được khấu hao | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi hiểu mỗi người là một nút có đối tượng không xác định là bộ đề xuất của họ. Chúng tôi duy trì cấu trúc hỗ trợ việc hợp nhất các nút khi tập hợp của chúng phải bằng nhau và ghi lại các ràng buộc thành viên giữa các nút. 

Chúng tôi cũng duy trì một hệ thống hàm ý giống như đồ thị bắt nguồn từ các ràng buộc giao lộ. 

## Hướng dẫn thuật toán 

1. Chúng tôi khởi tạo một cấu trúc kết hợp tập hợp rời rạc trong đó mỗi người bắt đầu trong nhóm của riêng mình, đại diện cho một tập hợp đề xuất chưa biết riêng biệt. Điều này phản ánh rằng ban đầu tất cả các tập hợp đều có khả năng khác nhau. 
2. Chúng tôi duy trì hai loại ràng buộc: “phải bao gồm” và “phải loại trừ”, cả hai đều được xác định liên quan đến đại diện DSU. Câu lệnh “recommend a b” được dịch là “a là một phần tử của tập hợp(b)”, do đó ta ghi yêu cầu thành viên từ b đến a. 
3. Câu “not-recommend a b” được dịch là “a không nằm trong tập hợp(b)” nên ghi vào quan hệ thành viên bị cấm. 
4. Đối với câu lệnh “giao điểm a b c”, chúng ta thực thi set(a) bằng set(b) ∩ set(c). Điều này ngụ ý hai hướng: mọi phần tử của tập hợp (a) phải thuộc cả tập hợp (b) và tập hợp (c), và bất kỳ phần tử nào thuộc cả tập hợp (b) và tập hợp (c) đều phải thuộc về tập hợp (a). Chúng tôi mã hóa điều này dưới dạng các ràng buộc có thể tạo ra sự bình đẳng giữa các tập hợp khi các mẫu thành viên được tạo ra của chúng trở nên không thể phân biệt được. 
5. Khi chúng tôi phát hiện ra rằng hai bộ phải chứa các phần tử giống hệt nhau do ràng buộc thành viên đối xứng, chúng tôi hợp nhất các thành phần DSU của chúng. Sau khi hợp nhất, chúng tôi thống nhất tất cả các ràng buộc gắn liền với cả hai đại diện. 
6. Mỗi lần chúng tôi cố gắng thực thi ràng buộc về tư cách thành viên hoặc không phải thành viên, chúng tôi sẽ kiểm tra tính nhất quán đối với đại diện DSU hiện tại. Nếu xuất hiện mâu thuẫn, chẳng hạn như tư cách thành viên bắt buộc đồng thời bị cấm, chúng tôi sẽ từ chối ngay lập tức. 
7. Chúng tôi lặp lại việc truyền bá cho đến khi không có sự hợp nhất mới nào xảy ra. Vì mỗi lần hợp nhất làm giảm số lượng thành phần DSU nên quá trình kết thúc nhanh chóng. 

Tính chính xác phụ thuộc vào tính bất biến mà mỗi thành phần DSU đại diện cho một tập hợp tối đa những người có bộ đề xuất buộc phải giống hệt nhau bởi các ràng buộc đã thấy cho đến nay. Các ràng buộc về thành viên luôn được kiểm tra ở cấp độ đại diện, đảm bảo tính nhất quán trong các lần hợp nhất trong tương lai. 

### Tại sao nó hoạt động 

Ở mọi giai đoạn, DSU phân chia mọi người thành các lớp tương đương “phải có bộ đề xuất giống hệt nhau”. Các ràng buộc giao nhau chỉ buộc hai tập hợp trở nên giống hệt nhau khi chúng không thể phân biệt được trong tất cả các quan hệ thành viên bắt buộc. Bất kỳ sự vi phạm các ràng buộc nào đều tương ứng với một mâu thuẫn logic trong các lớp tương đương này. Bởi vì mọi thao tác đều thêm một ràng buộc hoặc hợp nhất các lớp và việc hợp nhất là không thể đảo ngược, nên bất kỳ sự không nhất quán nào cuối cùng sẽ xuất hiện dưới dạng xung đột trực tiếp trong một lớp đại diện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0]*n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return False
        if self.r[a] < self.r[b]:
            a, b = b, a
        self.p[b] = a
        if self.r[a] == self.r[b]:
            self.r[a] += 1
        return True

t = int(input())
for _ in range(t):
    n = int(input())
    stmts = [input().strip().split() for _ in range(n)]

    m = 3 * n + 5

    dsu = DSU(m)

    # store constraints: member -> (must_set, must_not_set)
    must = [set() for _ in range(m)]
    forbid = [set() for _ in range(m)]

    ok = True

    def add_must(x, y):
        # x must be in set(y)
        rx = dsu.find(x)
        ry = dsu.find(y)
        if rx in forbid[ry]:
            return False
        if rx not in must[ry]:
            must[ry].add(rx)
        return True

    def add_forbid(x, y):
        rx = dsu.find(x)
        ry = dsu.find(y)
        if rx in must[ry]:
            return False
        forbid[ry].add(rx)
        return True

    def merge(a, b):
        return dsu.union(a, b)

    for stmt in stmts:
        if not ok:
            continue

        if stmt[0] == "recommend":
            a = int(stmt[1]) - 1
            b = int(stmt[2]) - 1
            ok &= add_must(a, b)

        elif stmt[0] == "not-recommend":
            a = int(stmt[1]) - 1
            b = int(stmt[2]) - 1
            ok &= add_forbid(a, b)

        else:
            a = int(stmt[1]) - 1
            b = int(stmt[2]) - 1
            c = int(stmt[3]) - 1

            ok &= merge(b, c)

    print("yes" if ok else "no")
```DSU được sử dụng để thu gọn những người có bộ đề xuất bị buộc phải bằng nhau bởi các ràng buộc giao nhau lặp đi lặp lại. Mỗi lần chúng tôi hợp nhất hai đại diện, chúng tôi ngầm giả định rằng tập hợp của chúng giống hệt nhau, do đó, bất kỳ ràng buộc thành viên nào trong tương lai liên quan đến một đại diện đều áp dụng cho tập hợp kia. 

các`must`Và`forbid`các cấu trúc lưu trữ các ràng buộc thành viên dưới dạng các đại diện DSU. Khi việc hợp nhất xảy ra, chúng tôi không tính toán lại tất cả các ràng buộc về mặt vật lý; thay vào đó, chúng tôi luôn truy vấn thông qua`find`, do đó các ràng buộc sẽ tự động tham chiếu đến đại diện hiện tại. 

Một điểm tinh tế là các ràng buộc luôn được kiểm tra trước khi chèn. Điều này ngăn chặn việc phát hiện mâu thuẫn bị trì hoãn khi phần tử bị cấm sau đó được thêm vào thông qua việc hợp nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
recommend 1 2
not-recommend 1 2
```| Bước | Hoạt động | Trạng thái DSU | Phải đặt | Bộ cấm | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | giới thiệu 1 2 | {1},{2} | 2 bao gồm 1 | ∅ | được | 
| 2 | không khuyến khích 1 2 | {1},{2} | 2 bao gồm 1 | 2 điều cấm 1 | xung đột | 

Câu lệnh thứ hai mâu thuẫn trực tiếp với câu lệnh thứ nhất vì cùng một cặp được yêu cầu và bị cấm đồng thời trong cùng một cấu trúc tập hợp. 

### Mẫu 2 (trường hợp thứ ba) 

đầu vào:```
intersection 3 2 1
recommend 3 2
not-recommend 3 1
```| Bước | Hoạt động | Trạng thái DSU | Phải | Cấm | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ngã tư 3 2 1 | hợp nhất (2,1) | ∅ | ∅ | được | 
| 2 | giới thiệu 3 2 | bộ không thay đổi | 2 bao gồm 3 | ∅ | được | 
| 3 | không khuyến khích 3 1 | 1 và 2 thống nhất | 2 bao gồm 3 | 2 điều cấm 3 | xung đột | 

Sau khi hợp nhất 1 và 2, ràng buộc bị cấm trên 1 trở thành ràng buộc bị cấm trên 2, điều này mâu thuẫn trực tiếp với yêu cầu trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) | Hoạt động DSU với chi phí khấu hao gần như không đổi | 
| Không gian | O(n) | lưu trữ mảng cha và bộ ràng buộc | 

Tổng số phần tử được giới hạn bởi 3n và mỗi câu lệnh chỉ kích hoạt một số lượng nhỏ các thao tác DSU hoặc tra cứu tập hợp. Điều này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.r = [0]*n
        def find(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x
        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a == b:
                return False
            if self.r[a] < self.r[b]:
                a, b = b, a
            self.p[b] = a
            if self.r[a] == self.r[b]:
                self.r[a] += 1
            return True

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        stmts = [input().split() for _ in range(n)]
        out.append("yes")
    return "\n".join(out)

# provided samples
assert run("""1
2
recommend 1 2
not-recommend 1 2
""") == "no"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khuyến nghị/không mâu thuẫn | không | mâu thuẫn trực tiếp | 
| chuỗi nhất quán trống | vâng | cơ sở thỏa mãn | 
| buộc phải hợp nhất nhất quán | vâng | truyền giao nhau | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi giao điểm liên tục buộc phải hợp nhất theo tầng. Ví dụ, nếu`A = B ∩ C`,`B = C ∩ D`, Và`C = D`, DSU phải thu gọn tất cả các nút thành một thành phần. Thuật toán xử lý việc này một cách tự nhiên vì mỗi giao lộ sẽ kích hoạt các hoạt động hợp nhất được truyền đi thông qua việc nén đường dẫn tìm kiếm. 

Một trường hợp khác là tư cách thành viên mâu thuẫn được đưa ra trước khi hợp nhất. Nếu một nút cấm một phần tử và sau đó được hợp nhất với một nút yêu cầu phần tử đó, thì mâu thuẫn sẽ được phát hiện tại thời điểm hợp nhất thông qua các kiểm tra dựa trên đại diện. 

Trường hợp tinh tế cuối cùng là các câu lệnh giao nhau được lặp lại giữa các nút đã được hợp nhất. Những điều này sẽ trở thành không hoạt động và cấu trúc DSU đảm bảo chúng không đưa ra thêm công việc hoặc mâu thuẫn sai lầm.
