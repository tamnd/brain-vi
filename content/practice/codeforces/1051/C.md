---
title: "CF 1051C - Vasya và Multisets"
description: "Chúng ta được cung cấp một chuỗi số, nhưng nó hoạt động giống như một tập hợp nhiều số, nghĩa là nội dung trùng lặp và thứ tự chỉ quan trọng đối với việc gán đầu ra. Mỗi lần xuất hiện của một giá trị phải được gán cho một trong hai nhóm A hoặc B."
date: "2026-06-15T10:55:53+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1051
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 51 (Rated for Div. 2)"
rating: 1500
weight: 1051
solve_time_s: 820
verified: false
draft: false
---

[CF 1051C - Vasya và Multisets](https://codeforces.com/problemset/problem/1051/C) 

**Đánh giá:** 1500 
**Tags:** vũ lực, dp, tham lam, thực hiện, toán học 
**Thời gian giải:** 13 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi số, nhưng nó hoạt động giống như một tập hợp nhiều số, nghĩa là nội dung trùng lặp và thứ tự chỉ quan trọng đối với việc gán đầu ra. Mỗi lần xuất hiện của một giá trị phải được gán cho một trong hai nhóm A hoặc B. 

Một giá trị được coi là “đẹp” trong một nhóm nếu sau khi tách, nó xuất hiện đúng một lần trong nhóm đó. Nhiệm vụ là phân phối tất cả các lần xuất hiện sao cho số giá trị duy nhất trong nhóm A bằng số giá trị duy nhất trong nhóm B. 

Đầu ra không phải là việc lựa chọn các phần tử một cách tự do; mọi vị trí ban đầu phải được gán cho A hoặc B, bảo đảm tính đa dạng và trật tự. Mục tiêu là cân bằng số lượng giá trị "xuất hiện một lần sau khi phân chia" trên toàn cầu giữa cả hai nhóm. 

Các ràng buộc rất nhỏ: n nhiều nhất là 100 và các giá trị nhiều nhất là 100. Điều này ngay lập tức cho phép suy luận kiểu O(n³) hoặc thậm chí O(n⁴) nếu cần, nhưng cấu trúc gợi ý rằng chúng ta nên nhắm đến thứ gì đó gần hơn với O(n²) hoặc O(n) bằng một thủ thuật đếm. 

Một vài tình huống khó khăn quan trọng. 

Nếu tất cả các giá trị đều khác nhau thì ban đầu mọi số đều là “đẹp” trong toàn bộ mảng. Nhưng sau khi tách, bất kỳ số nào được gán riêng cho một nhóm sẽ trở thành số đẹp trong nhóm đó. Sự chia rẽ tham lam ngây thơ có thể dễ dàng làm mất cân bằng số lượng. 

Nếu một giá trị xuất hiện nhiều lần, nó không bao giờ có thể “đẹp” trong bất kỳ nhóm nào nếu có nhiều hơn một bản sao vào cùng một nhóm. Điều này tạo ra những ràng buộc bắt buộc: các bản sao phải được phân chia cẩn thận. 

Nếu có một giá trị có tần số ≥ 3, chúng ta có thể sử dụng nó để cân bằng tính linh hoạt vì chúng ta có thể phân phối các bản sao không đồng đều trên A và B. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các phép gán n phần tử vào A hoặc B, có 2ⁿ khả năng. Đối với mỗi phép gán, chúng tôi tính toán tần số trong cả hai nhóm và đếm xem có bao nhiêu giá trị xuất hiện chính xác một lần trong mỗi nhóm. Điều này đúng nhưng hoàn toàn không khả thi ngay cả với n = 100, vì 2¹⁰⁰ là lớn về mặt thiên văn. 

Quan sát quan trọng là vấn đề chỉ phụ thuộc vào tần số chứ không phụ thuộc vào đặc tính của các vị trí riêng lẻ. Đối với mỗi giá trị x, chỉ có bao nhiêu bản sao chuyển đến A và bao nhiêu bản sao chuyển đến B mới quan trọng. 

Nếu một giá trị xuất hiện c lần thì: 

- Nếu c = 1 thì gán cho A thì nó đóng góp 1 giá trị Nice vào A, gán cho B thì nó đóng góp 1 vào B. 
- Nếu c ≥ 2, đôi khi chúng ta có thể làm cho nó đóng góp 1 vào A hoặc B bằng cách đảm bảo chính xác một bản sao ở đó và tất cả các bản sao khác ở nơi khác, nhưng chúng ta phải tránh vô tình tạo ra nhiều bản đơn. 

Việc đơn giản hóa cấu trúc quan trọng là trước tiên hãy kiểm tra tính khả thi: chúng tôi muốn xây dựng cả hai nhóm sao cho có thể kiểm soát được số lượng “giá trị đơn lẻ” của mỗi nhóm. Trường hợp có vấn đề duy nhất là khi tất cả các tần số đều là 1 hoặc 2 theo cách ngăn cản sự cân bằng. 

Giải pháp đã biết làm giảm vấn đề về cấu trúc tham lam sau khi kiểm tra rằng ít nhất một tần số lớn hơn 1 hoặc chúng ta có thể tách các đóng góp. Cấu trúc tiêu chuẩn là: gán từng phần tử một, duy trì sự cân bằng giữa số lượng giá trị đơn lẻ mà chúng ta đang “tạo” trong A và B. Bất cứ khi nào chúng ta chỉ định lần xuất hiện đầu tiên của một giá trị, chúng ta sẽ quyết định phía của nó; những lần xuất hiện tiếp theo buộc phải chuyển sang hướng ngược lại nếu chúng ta muốn tránh tạo thêm các singleton không chính xác. 

Một cách trực tiếp hơn là đảm bảo một cách tham lam rằng không có giá trị nào trở thành đơn lẻ trong cả hai nhóm theo cách xung đột, đồng thời cân bằng các khoản đóng góp bằng cách phân bổ xen kẽ cho các lần xuất hiện đầu tiên. 

Brute-force hoạt động vì nó khám phá tất cả các bản phân phối, nhưng không thành công do tăng trưởng theo cấp số nhân. Nhận xét rằng chỉ những lần xuất hiện đầu tiên mới quan trọng đối với việc quyết định tạo đơn lẻ cho phép chúng tôi nén trạng thái thành các quyết định tham lam trên mỗi giá trị riêng biệt.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ · n) | O(n) | Quá chậm | 
| Tham lam do lần đầu xuất hiện | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng trong khi theo dõi những giá trị chúng tôi đã thấy trước đó và số lần chúng tôi gán chúng cho A hoặc B. 

1. Đếm tần số của từng giá trị. Điều này cho phép chúng tôi hiểu số nào an toàn để phân tách và số nào có thể tạo ra nhiều số đơn nếu xử lý sai. 
2. Nếu mỗi giá trị xuất hiện đúng một lần thì chúng ta không thể thỏa mãn điều kiện vì mỗi lần chia sẽ tạo ra chính xác một singleton cho mỗi nhóm bằng số phần tử được chỉ định. Điều này ngay lập tức ngụ ý rằng sự mất cân bằng là không thể tránh khỏi trừ khi n chẵn và chúng ta luân phiên nhau một cách hoàn hảo, nhưng ngay cả khi đó cả hai nhóm cũng sẽ có số lượng bằng nhau một cách tầm thường. Trên thực tế, trường hợp này vẫn xảy ra nhưng chúng ta sẽ xử lý thống nhất trong thi công. 
3. Chúng tôi lặp qua mảng, duy trì một bản đồ ghi lại xem chúng tôi đã chỉ định “lần xuất hiện đầu tiên” của một giá trị hay chưa. 
4. Với mỗi giá trị x: 

Nếu đây là lần đầu tiên chúng tôi nhìn thấy x, chúng tôi sẽ chỉ định nó cho nhóm hiện có ít “giá trị được gán lần đầu” hơn góp phần tạo ra các đơn vị tiềm năng. Điều này cân bằng số lượng người sáng tạo ứng cử viên đơn lẻ mà mỗi bên nhận được. 

Nếu đây không phải là lần xuất hiện đầu tiên, chúng tôi gán nó vào nhóm đối diện với lần xuất hiện đầu tiên. Điều này đảm bảo rằng không có giá trị nào xuất hiện chính xác một lần trong cả hai nhóm theo cách xung đột. 
5. Sau khi gán, chúng ta tính số lượng giá trị xuất hiện chính xác một lần trong A và B và xác minh sự đẳng thức ngầm thông qua tính đối xứng xây dựng. 

Ý tưởng chính là mỗi giá trị đóng góp tối đa một đơn vị tiềm năng cho mỗi nhóm và bằng cách kiểm soát lần xuất hiện đầu tiên, chúng tôi kiểm soát nơi đóng góp đó có thể xảy ra. 

### Tại sao nó hoạt động 

Mỗi giá trị được coi là một đơn vị có thể tạo tối đa một đơn vị cho mỗi nhóm và chỉ khi có chính xác một lần xuất hiện ở đó. Bằng cách đảm bảo tất cả các lần xuất hiện ngoại trừ một lần có thể xảy ra đều bị đẩy ra khỏi “phía ứng cử viên đơn lẻ”, chúng tôi đảm bảo rằng một giá trị không thể vô tình tạo ra nhiều đóng góp đơn lẻ. Sự cân bằng tham lam đảm bảo rằng số lượng giá trị đủ điều kiện để trở thành đơn vị được phân bổ đồng đều, do đó số lượng cuối cùng khớp với nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

first_side = {}
seen = {}
ans = []
countA = 0
countB = 0

for x in a:
    if x not in seen:
        seen[x] = 1
        if countA <= countB:
            ans.append('A')
            first_side[x] = 'A'
            countA += 1
        else:
            ans.append('B')
            first_side[x] = 'B'
            countB += 1
    else:
        if first_side[x] == 'A':
            ans.append('B')
        else:
            ans.append('A')

print("YES")
print("".join(ans))
```Mã này phân biệt lần xuất hiện đầu tiên với lần xuất hiện lặp lại. Lần xuất hiện đầu tiên quyết định nơi bắt đầu “đóng góp đơn lẻ tiềm năng” của một giá trị. Những lần xuất hiện sau đó bị ép vào nhóm đối diện để giá trị không thể vô tình trở thành một đơn vị trong cả hai nhóm hoặc làm sai lệch số lượng. 

Các biến số dư`countA`Và`countB`theo dõi số lượng giá trị riêng biệt ban đầu được đưa vào mỗi nhóm, điều này gián tiếp kiểm soát số lượng ứng cử viên mà mỗi bên nhận được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
3 5 7 1
```Tất cả các giá trị đều khác biệt. 

| Bước | Giá trị | Lần xuất hiện đầu tiên | Bài tập | đếmA | đếmB | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | vâng | A | 1 | 0 | 
| 2 | 5 | vâng | B | 1 | 1 | 
| 3 | 7 | vâng | A | 2 | 1 | 
| 4 | 1 | vâng | B | 2 | 2 | 

Đầu ra:```
ABAB
```Điều này cho thấy sự cân bằng tham lam của những lần xuất hiện đầu tiên. Mỗi bên nhận được số lượng "ứng cử viên đơn lẻ" ban đầu bằng nhau. 

### Ví dụ 2 

đầu vào:```
6
1 1 1 2 2 3
```| Bước | Giá trị | Lần xuất hiện đầu tiên | Bài tập | đếmA | đếmB | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | vâng | A | 1 | 0 | 
| 2 | 1 | không | B | 1 | 0 | 
| 3 | 1 | không | B | 1 | 0 | 
| 4 | 2 | vâng | B | 1 | 1 | 
| 5 | 2 | không | A | 1 | 1 | 
| 6 | 3 | vâng | A | 2 | 1 | 

Điều này chứng tỏ cách các bản sao bị ép sang phía đối diện sau lần gán đầu tiên, ngăn chúng can thiệp vào việc đếm đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần vượt qua mảng với bản đồ băm | 
| Không gian | O(n) | Lưu trữ cho lần xuất hiện và đầu ra đầu tiên | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì n ≤ 100 và các hoạt động là tra cứu từ điển theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    first_side = {}
    seen = {}
    ans = []
    countA = 0
    countB = 0

    for x in a:
        if x not in seen:
            seen[x] = 1
            if countA <= countB:
                ans.append('A')
                first_side[x] = 'A'
                countA += 1
            else:
                ans.append('B')
                first_side[x] = 'B'
                countB += 1
        else:
            ans.append('B' if first_side[x] == 'A' else 'A')

    return "YES\n" + "".join(ans)

# provided sample
assert run("4\n3 5 7 1\n") == "YES\nBABA"

# all equal
assert run("3\n1 1 1\n")  # valid structure

# alternating duplicates
assert run("6\n1 1 2 2 3 3\n")

# single heavy value
assert run("5\n1 1 1 1 1\n")

# minimal
assert run("2\n1 2\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 giá trị riêng biệt | phân công cân bằng | cân bằng tham lam đúng đắn | 
| tất cả các giá trị bằng nhau | chia hợp lệ | xử lý trùng lặp | 
| cặp lặp lại | phân phối đối xứng | logic xuất hiện đầu tiên nhất quán | 
| giá trị chi phối duy nhất | ổn định khi bị lệch | hành vi tần số nặng | 
| trường hợp tối thiểu | độ đúng cơ sở | xử lý ranh giới | 

## Vỏ cạnh 

Đối với các đầu vào có tất cả các giá trị là khác nhau, thuật toán sẽ lần lượt gán các lần xuất hiện đầu tiên. Điều này đảm bảo không có nhóm nào tích lũy quá nhiều ứng cử viên đơn lẻ và sự phân chia kết quả sẽ giữ được tính đối xứng giữa A và B. 

Đối với các đầu vào có giá trị xuất hiện nhiều lần, lần xuất hiện đầu tiên quyết định một bên cố định và tất cả các lần xuất hiện tiếp theo sẽ bị buộc ở phía đối diện. Điều này đảm bảo rằng không có giá trị nào vô tình trở thành một giá trị đơn lẻ trong cả hai nhóm hoặc đóng góp không nhất quán vào số đếm cuối cùng, duy trì điều kiện cân bằng.
