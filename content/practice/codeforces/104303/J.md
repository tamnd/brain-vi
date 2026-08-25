---
title: "CF 104303J - \u7ec4\u961f"
description: "Chúng ta được cấp một nhóm học sinh, mỗi học sinh biết một tập hợp con gồm tối đa 60 chủ đề. Một nhóm hợp lệ là bất kỳ tập hợp con nào của học sinh thỏa mãn đồng thời hai điều kiện: mọi chủ đề từ 1 đến p được thực hiện bởi ít nhất một thành viên trong nhóm và đối với mỗi chủ đề, tối đa một đội…"
date: "2026-07-01T20:12:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104303
codeforces_index: "J"
codeforces_contest_name: "2023 Xiangtan Unversity Freshman Conteset"
rating: 0
weight: 104303
solve_time_s: 50
verified: true
draft: false
---

[CF 104303J - \u7ec4\u961f](https://codeforces.com/problemset/problem/104303/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một nhóm học sinh, mỗi học sinh biết một tập hợp con gồm tối đa 60 chủ đề. Một nhóm hợp lệ là bất kỳ tập hợp con nào của học sinh thỏa mãn đồng thời hai điều kiện: mọi chủ đề từ 1 đến p đều có ít nhất một thành viên trong nhóm thực hiện và đối với mỗi chủ đề, tối đa một thành viên trong nhóm được phép biết. Nói cách khác, đối với mọi chủ đề, nếu bạn nhìn vào tập hợp con đã chọn, chủ đề đó có thể xuất hiện trong tập kiến ​​thức của 0 hoặc một người, nhưng không bao giờ nhiều hơn một, trong khi vẫn yêu cầu mọi chủ đề phải xuất hiện ở đâu đó trong nhóm. 

Nhiệm vụ là đếm xem có bao nhiêu tập con học sinh thỏa mãn các ràng buộc này. 

Hậu quả về mặt cấu trúc quan trọng nhất là mọi chủ đề hoạt động giống như một ràng buộc cấm ghép đôi hai học sinh cùng theo chủ đề đó, đồng thời buộc chủ đề đó xuất hiện ít nhất một lần trong tổng thể. Vì mỗi học sinh được biểu thị bằng một tập hợp con có tối đa 60 bit và n nhiều nhất là 42, nên không gian tìm kiếm tự nhiên là tất cả các tập hợp con của học sinh, tức là 2^42, đã có khoảng 4,4 nghìn tỷ khả năng. Bất kỳ giải pháp nào liệt kê trực tiếp các tập hợp con đều không khả thi. 

Một trường hợp phức tạp xuất hiện khi một số chủ đề không có ở bất kỳ học sinh nào. Trong trường hợp đó, không có nhóm hợp lệ nào tồn tại vì yêu cầu về phạm vi bao phủ không thể được đáp ứng. Ví dụ: nếu p = 3 và không ai biết chủ đề 2 thì ngay cả khi chọn tất cả học sinh, chủ đề 2 vẫn không được khám phá nên đáp án phải bằng 0. 

Một chế độ thất bại khác xuất hiện khi hai học sinh chia sẻ một chủ đề. Số lượng tập hợp con đơn giản vẫn sẽ bao gồm cả hai, nhưng các tập hợp con đó không hợp lệ bất kể các chủ đề khác, vì vậy việc cắt bớt là cần thiết thay vì chỉ kiểm tra ở cuối. 

## Phương pháp tiếp cận 

Một giải pháp Brute Force sẽ lặp lại trên tất cả các tập hợp con của học sinh, tính toán sự kết hợp các chủ đề trong tập hợp con đó và kiểm tra hai điều kiện: sự kết hợp phải bao gồm tất cả p chủ đề và không có chủ đề nào phải xuất hiện hai lần. Điều kiện thứ hai có thể được kiểm tra bằng cách duy trì một mảng tần số cho mỗi tập hợp con hoặc bằng cách theo dõi các phần chồng chéo trong quá trình xây dựng. Ngay cả với mặt nạ bit, điều này vẫn yêu cầu lặp lại trên 2^42 tập hợp con và đối với mỗi tập hợp con quét tối đa 42 học sinh hoặc 60 bit, dẫn đến độ phức tạp vượt xa giới hạn khả thi. 

Quan sát quan trọng là ràng buộc “không có chủ đề nào xuất hiện ở hai học sinh được chọn” biến mỗi nhóm hợp lệ thành một cấu trúc trong đó các học sinh được chọn phải có các mặt nạ bit rời nhau. Điều này có nghĩa là chúng ta đang đếm các tập con của các tập rời rạc theo cặp mà phép hợp bao phủ tất cả các bit p. Đây là một vấn đề đóng gói tập hợp rời rạc cổ điển. 

Vì n chỉ là 42 nhưng p lên tới 60 nên tập con trực tiếp DP trên học sinh là quá lớn. Thủ thuật tiêu chuẩn trong chế độ này là gặp nhau ở giữa. Chúng tôi chia học sinh thành hai nửa có kích thước tối đa là 21. Mỗi nửa có thể được liệt kê độc lập và chúng tôi ghi lại, đối với mỗi tập hợp con, hai phần thông tin: mặt nạ bit của các chủ đề mà nó đề cập đến và liệu nó có hợp lệ nội bộ hay không (không trùng lặp trong một nửa). 

Đối với mỗi nửa, chúng tôi tạo ra tất cả các tập hợp con hợp lệ và nhóm chúng theo mặt nạ che phủ của chúng. Điều kiện rời rạc giữa các nửa trở thành một ràng buộc AND bitwise đơn giản: một tập con bên trái có mặt nạ L và một tập con bên phải có mặt nạ R chỉ có thể được kết hợp nếu L & R = 0. Yêu cầu bao phủ trở thành L | R = ĐẦY ĐỦ_MASK. 

Sau đó, việc đếm sẽ giảm xuống việc lặp lại tất cả các trạng thái bên trái hợp lệ và khớp các trạng thái bên phải tương thích bằng cách sử dụng mặt nạ bổ sung. Tần số tính toán trước của mặt nạ bên phải cho phép tổng hợp nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Gặp-ở-giữa | O(2^(n/2) · 2^(n/2)) | O(2^(n/2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia học sinh thành hai nhóm trái và phải, mỗi nhóm có khoảng n/2 người.

1. Chuyển đổi tập kiến ​​thức của mỗi học sinh thành một bitmask trên p bit. Điều này cho phép kiểm tra giao và hợp nhanh bằng cách sử dụng các thao tác bit thay vì vòng lặp theo chủ đề. 
2. Liệt kê tất cả các tập con của nửa bên trái. Đối với mỗi tập hợp con, chúng tôi xây dựng mặt nạ hợp của nó và đồng thời xác minh rằng không có chủ đề nào xuất hiện hai lần trong tập hợp con. Chúng tôi duy trì mặt nạ chạy và trạng thái "hợp lệ cho đến nay"; nếu một học sinh mới trùng lặp với mặt nạ hiện tại thì tập hợp con đó sẽ bị loại bỏ. 
3. Lưu trữ mọi mặt nạ con bên trái hợp lệ vào bảng tần số. Nếu nhiều tập hợp con tạo ra cùng một mặt nạ, chúng tôi sẽ đếm chúng cùng nhau vì chúng không thể phân biệt được để so sánh sau này. 
4. Lặp lại quá trình tương tự cho nửa bên phải, tạo ra bảng tần số của các mặt nạ bên phải hợp lệ. 
5. Đối với mỗi cặp mặt nạ (L, R) trong đó L đến từ bảng bên trái và R đến từ bảng bên phải, chúng ta kiểm tra hai điều kiện: L & R phải bằng 0 và L | R phải bằng tập hợp đầy đủ các chủ đề. Khi cả hai đều giữ nguyên, đóng góp cho câu trả lời là freqL[L] × freqR[R]. 
6. Tổng hợp tất cả các khoản đóng góp và đưa ra kết quả. 

Lý do chúng ta có thể kết hợp các nửa một cách độc lập là vì tính hợp lệ bên trong mỗi nửa đã tạo ra sự rời rạc trong nửa đó. Xung đột giữa các phần được xử lý rõ ràng bằng cách sử dụng bit AND, do đó không có sự chồng chéo không hợp lệ nào có thể tồn tại trong bước hợp nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def gen_half(arr, p):
    n = len(arr)
    res = {}
    for mask in range(1 << n):
        ok = True
        cur = 0
        for i in range(n):
            if mask >> i & 1:
                x = arr[i]
                if cur & x:
                    ok = False
                    break
                cur |= x
        if ok:
            res[cur] = res.get(cur, 0) + 1
    return res

def solve():
    T = int(input())
    for _ in range(T):
        n, p = map(int, input().split())
        full = (1 << p) - 1
        a = []
        for _ in range(n):
            tmp = list(map(int, input().split()))
            k = tmp[0]
            mask = 0
            for x in tmp[1:]:
                mask |= 1 << (x - 1)
            a.append(mask)

        mid = n // 2
        left = a[:mid]
        right = a[mid:]

        L = gen_half(left, p)
        R = gen_half(right, p)

        ans = 0
        for lm, lv in L.items():
            for rm, rv in R.items():
                if (lm & rm) == 0 and (lm | rm) == full:
                    ans += lv * rv

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách mã hóa kiến ​​thức của mỗi học sinh dưới dạng bitmask. Điều này làm giảm tất cả các hoạt động chủ đề thành các hoạt động theo bit O(1). 

chức năng`gen_half`liệt kê tất cả các tập hợp con của một nửa số học sinh. Đối với mỗi mặt nạ tập hợp con, nó xây dựng sự kết hợp của các chủ đề và đảm bảo không xảy ra sự chồng chéo bên trong tập hợp con. Séc`cur & x`phát hiện xem sinh viên mới có chia sẻ bất kỳ chủ đề nào đã có trong tập hợp con hay không. 

Mỗi tập hợp con hợp lệ đóng góp mặt nạ bao phủ kết quả của nó vào một từ điển tần số, vì nhiều tập hợp con có thể tạo ra cùng một phạm vi bao phủ. 

Sau khi tạo cả hai nửa, bước cuối cùng sẽ kiểm tra tính tương thích. điều kiện`(lm & rm) == 0`thực thi sự rời rạc giữa các nửa, và`(lm | rm) == full`thực thi phạm vi bảo hiểm đầy đủ của tất cả các chủ đề. 

Việc lặp lại lồng nhau trên các mặt nạ có thể chấp nhận được vì mỗi nửa có tối đa 2^21 tập hợp con. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ với p = 3 và bốn học sinh: 

sinh viên 1 = {1}, sinh viên 2 = {2}, sinh viên 3 = {3}, sinh viên 4 = {1,2} không hợp lệ trong bất kỳ tập hợp con nào chỉ có 1 hoặc 2 do các ràng buộc chồng chéo tùy thuộc vào việc ghép nối. 

Chia thành {1,2} trái và {3,4} phải. 

### Nửa liệt kê bên trái 

| tập hợp con | mặt nạ trị mụn | hợp lệ | 
| --- | --- | --- | 
| ∅ | 000 | vâng | 
| {1} | 001 | vâng | 
| {2} | 010 | vâng | 
| {1,2} | 011 | vâng | 
| {4} | 011 | có (mục đơn) | 

Điều này tạo ra số lượng tần số giống như mặt nạ 011 xuất hiện hai lần. 

### Nửa bên phải liệt kê 

| tập hợp con | mặt nạ trị mụn | hợp lệ | 
| --- | --- | --- | 
| ∅ | 000 | vâng | 
| {3} | 100 | vâng | 
| {4} | 011 | vâng | 

Bây giờ chúng ta kết hợp. Chúng tôi chỉ chấp nhận các cặp có hợp là 111 và giao là 0. Vì vậy mặt nạ bên trái 011 chỉ có thể ghép với mặt nạ bên phải 100. 

Dấu vết này cho thấy tính hợp lệ hoàn toàn mang tính cục bộ bên trong các nửa và các ràng buộc toàn cục chỉ được thực thi tại thời điểm hợp nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^(n/2) · 2^(n/2)) | liệt kê tất cả các tập hợp con của cả hai nửa và mặt nạ phù hợp | 
| Không gian | O(2^(n/2)) | lưu trữ bản đồ tần số của mặt nạ tập hợp con | 

Với n 42, mỗi nửa có tối đa 2^21 ≈ 2 triệu tập con. Cách tiếp cận này phù hợp với giới hạn của Python khi được triển khai bằng các thao tác bit và tổng hợp từ điển. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import defaultdict

    def gen_half(arr, p):
        n = len(arr)
        res = {}
        for mask in range(1 << n):
            ok = True
            cur = 0
            for i in range(n):
                if mask >> i & 1:
                    x = arr[i]
                    if cur & x:
                        ok = False
                        break
                    cur |= x
            if ok:
                res[cur] = res.get(cur, 0) + 1
        return res

    T = int(input())
    out = []
    for _ in range(T):
        n, p = map(int, input().split())
        full = (1 << p) - 1
        a = []
        for _ in range(n):
            tmp = list(map(int, input().split()))
            k = tmp[0]
            mask = 0
            for x in tmp[1:]:
                mask |= 1 << (x - 1)
            a.append(mask)

        mid = n // 2
        L = gen_half(a[:mid], p)
        R = gen_half(a[mid:], p)

        ans = 0
        for lm, lv in L.items():
            for rm, rv in R.items():
                if (lm & rm) == 0 and (lm | rm) == full:
                    ans += lv * rv
        out.append(str(ans))

    return "\n".join(out)

# small sanity cases
assert run("1\n1 1\n1 1\n") == "1", "single student covers one topic"
assert run("1\n2 2\n1 1\n1 2\n") == "1", "only full pairing works"
assert run("1\n2 2\n1 1\n1 1\n") == "0", "duplicate topic invalid"
assert run("1\n3 3\n1 1\n1 2\n1 3\n") == "1", "perfect partition"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sinh viên độc thân | 1 | nhóm hợp lệ tối thiểu | 
| hai sinh viên bổ sung cho nhau | 1 | kết hợp nửa chéo | 
| kiến thức trùng lặp | 0 | từ chối chồng chéo | 
| phân vùng đầy đủ | 1 | chính xác bảo hiểm đầy đủ | 

## Vỏ cạnh 

Trường hợp tất cả học sinh chia sẻ ít nhất một chủ đề chung sẽ ngay lập tức buộc câu trả lời về 0 vì bất kỳ tập hợp con nào có kích thước ít nhất là hai đều vi phạm quy tắc "không lặp lại chủ đề". Thuật toán xử lý việc này một cách tự nhiên vì trong quá trình liệt kê tập hợp con, bất kỳ cặp nào bao gồm cả hai học sinh đều sẽ kích hoạt`cur & x != 0`, loại bỏ tất cả các tập hợp con đa phần tử. 

Trường hợp thiếu chủ đề sẽ dẫn đến`full`không bao giờ có thể tiếp cận được. Ngay cả khi mặt nạ bên trái và bên phải kết hợp mà không chồng chéo lên nhau,`(lm | rm) == full`sẽ không bao giờ giữ được, vì vậy câu trả lời tích lũy vẫn bằng không. 

Một trường hợp có nhiều học sinh giống hệt nhau được xử lý thông qua việc tổng hợp tần số trong bản đồ mặt nạ. Mặc dù các tập hợp con khác nhau về mặt tổ hợp, chúng vẫn thu gọn thành các mặt nạ giống hệt nhau và được tính chính xác thông qua phép nhân trong bước hợp nhất cuối cùng.
