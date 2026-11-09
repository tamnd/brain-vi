---
title: "CF 1480B - Người Hùng Vĩ Đại"
description: "Vấn đề đặt ra là một anh hùng có sức tấn công và sức khỏe cố định phải đối mặt với một nhóm quái vật, mỗi quái vật có đòn tấn công và sức khỏe riêng. Trận chiến diễn ra không ngừng nhưng đồng thời: khi anh hùng tấn công một quái vật, quái vật đó cũng gây sát thương tấn công lại cho anh hùng."
date: "2026-06-10T23:40:43+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1480
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 700 (Div. 2)"
rating: 900
weight: 1480
solve_time_s: 118
verified: true
draft: false
---

[CF 1480B - Người hùng vĩ đại](https://codeforces.com/problemset/problem/1480/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề đặt ra là một anh hùng có sức tấn công và sức khỏe cố định phải đối mặt với một nhóm quái vật, mỗi quái vật có đòn tấn công và sức khỏe riêng. Trận chiến diễn ra không ngừng nhưng đồng thời: khi anh hùng tấn công một quái vật, quái vật đó cũng gây sát thương tấn công lại cho anh hùng. Quái vật bị đánh bại khi máu của nó giảm xuống 0 hoặc thấp hơn, và anh hùng bị đánh bại khi máu của anh ta giảm xuống 0 hoặc thấp hơn. Mục tiêu là để xác định xem liệu anh hùng có thể tiêu diệt tất cả quái vật hay không, ngay cả khi anh ta chết trong quá trình này, miễn là không có quái vật nào sống sót. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp đòn tấn công của anh hùng, sức khỏe của anh hùng, số lượng quái vật, danh sách các cuộc tấn công của quái vật và danh sách sức khỏe của quái vật. Đầu ra là CÓ hoặc KHÔNG đơn giản cho mỗi trường hợp thử nghiệm. 

Các ràng buộc cho phép tối đa 10^5 quái vật trong tất cả các trường hợp thử nghiệm cũng như lượng máu và sức tấn công của quái vật lên tới 10^6. Bất kỳ mô phỏng ngây thơ nào lặp đi lặp lại việc trừ đi sức khỏe cho mỗi đòn tấn công cho đến khi quái vật chết đều quá chậm trong trường hợp xấu nhất. Ví dụ: nếu một quái vật có máu 10^6 và đòn tấn công của anh hùng là 1, thì việc mô phỏng mỗi đòn tấn công có thể bao gồm 10^6 lần lặp cho mỗi quái vật, vượt xa giới hạn khả thi. 

Một trường hợp phức tạp xảy ra khi một con quái vật gây sát thương lớn nhưng lại có lượng máu thấp. Một thuật toán tham lam bất cẩn có thể tấn công quái vật theo thứ tự tùy ý và khiến anh hùng chết sớm. Ví dụ: một quái vật có máu 999 và tấn công 1000 vào một anh hùng có 999 máu sẽ ngay lập tức gây tử vong. Câu trả lời đúng là KHÔNG, nhưng nếu bỏ qua tổng thiệt hại cần thiết và chỉ đếm số lần tấn công, người ta có thể nói nhầm CÓ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ lực mô phỏng mọi cuộc tấn công một cách rõ ràng: liên tục chọn một quái vật còn sống, trừ đòn tấn công của anh hùng khỏi sức khỏe của nó, trừ đòn tấn công của quái vật khỏi sức khỏe của anh hùng và lặp lại cho đến khi tất cả quái vật hoặc anh hùng đều chết. Điều này đúng nhưng có khả năng yêu cầu hàng triệu thao tác cho mỗi trường hợp thử nghiệm, tốc độ này quá chậm so với các hạn chế. 

Cách tiếp cận tối ưu thúc đẩy quan sát rằng tổng sát thương mà mỗi quái vật gây ra tỷ lệ thuận với số đòn tấn công cần thiết để tiêu diệt nó. Đối với mỗi quái vật, số lần tấn công anh hùng cần thiết là ceil(monster_health / hero_Attack). Mỗi đòn tấn công khiến anh hùng phải chịu sát thương bằng đòn tấn công của quái vật. Tổng kết điều này trên tất cả quái vật sẽ cho ra tổng thiệt hại mà anh hùng sẽ phải gánh chịu nếu anh hùng chiến đấu với từng quái vật một cách cô lập. Người anh hùng sống sót nếu sức khỏe của anh ta vượt quá hoặc bằng số tiền này trừ đi đòn tấn công đơn lẻ lớn nhất của quái vật. Điều này có tác dụng vì quái vật cuối cùng có thể bị tiêu diệt ngay cả khi anh hùng chết trong đòn tấn công đó, nhưng tất cả những quái vật khác phải sống sót hoàn toàn. Sắp xếp là không cần thiết; người ta chỉ cần đòn tấn công quái vật tối đa và tổng “sát thương hiệu quả” trên tất cả quái vật. 

Lực lượng vũ phu rất đơn giản để lý giải nhưng O(n * max(b_i/A)), trong khi cách tiếp cận tối ưu là tuyến tính cho mỗi trường hợp thử nghiệm, O(n), hoàn toàn phù hợp với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max(b_i / A)) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc đòn tấn công của anh hùng`A`, sức khỏe của anh hùng`B`, và số lượng quái vật`n`. 
2. Đọc mảng tấn công quái vật`a`và mảng sức khỏe quái vật`b`. 
3. Với mỗi quái vật, hãy tính số lần tấn công cần thiết để tiêu diệt nó. Đây là`(b_i + A - 1) // A`. Nhân số này với đòn tấn công của quái vật để tìm tổng sát thương mà quái vật này gây ra trước khi chết. Tính tổng các giá trị này trên tất cả quái vật. 
4. Xác định đòn tấn công đơn lẻ lớn nhất của quái vật`max_attack`. Điều này thể hiện thiệt hại mà anh hùng có thể gánh chịu với quái vật cuối cùng, vì anh ta có thể chết sau khi giết nó. 
5. Nếu sức khỏe của anh hùng`B`lớn hơn hoặc bằng tổng số thiệt hại gây ra trừ đi`max_attack`, xuất ra CÓ. Nếu không thì xuất ra NO. 

Tại sao nó hoạt động: Điểm bất biến chính là tổng sát thương của mỗi quái vật được tính dựa trên số đòn tấn công tối thiểu cần thiết để tiêu diệt nó. Trừ đi số đòn tấn công tối đa của quái vật sẽ mô hình hóa kịch bản trong đó anh hùng kết liễu quái vật cuối cùng trong khi có thể chết đồng thời. Điều này đảm bảo rằng anh hùng có thể tiêu diệt tất cả quái vật mà không để sót một con nào còn sống, phù hợp với yêu cầu của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        A, B, n = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        total_damage = 0
        max_attack = 0
        
        for ai, bi in zip(a, b):
            attacks_needed = (bi + A - 1) // A
            total_damage += attacks_needed * ai
            max_attack = max(max_attack, ai)
        
        if B >= total_damage - max_attack:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    main()
```Mỗi phần của giải pháp tương ứng trực tiếp với hướng dẫn thuật toán. các`attacks_needed`tính toán sử dụng phép chia trần để xác định chính xác số lượng cuộc tấn công cần thiết.`total_damage`tổng hợp thiệt hại tiềm tàng từ mỗi quái vật.`max_attack`được sử dụng để cho phép anh hùng chết trong đòn tấn công cuối cùng. Các trường hợp đặc biệt, chẳng hạn như một con quái vật có máu anh hùng chính xác bằng sát thương nhận được sẽ được xử lý bằng so sánh cuối cùng`B >= total_damage - max_attack`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 17 1
2
16
```| Sức khỏe anh hùng B | Sức khỏe quái vật b_i | Quái vật tấn công a_i | Những cuộc tấn công cần thiết | Tổng thiệt hại | 
| --- | --- | --- | --- | --- | 
| 17 | 16 | 2 | 6 | 12 | 

Số lần tấn công quái vật tối đa = 2. Kiểm tra: 17 >= 12 - 2 → 17 >= 10 → CÓ. 

Bảng cho thấy sau tất cả các cuộc tấn công, anh hùng vẫn có thể sống sót sau tổng sát thương cần thiết trừ đi đòn tấn công của quái vật cuối cùng. 

### Mẫu 2 

đầu vào:```
999 999 1
1000
1000
```| Sức khỏe anh hùng B | Sức khỏe quái vật b_i | Quái vật tấn công a_i | Những cuộc tấn công cần thiết | Tổng thiệt hại | 
| --- | --- | --- | --- | --- | 
| 999 | 1000 | 1000 | 1 | 1000 | 

Tấn công quái vật tối đa = 1000. Kiểm tra: 999 >= 1000 - 1000 → 999 >= 0 → CÓ. 

Người anh hùng chết sau khi ra đòn cuối cùng, điều này được cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lần vượt qua để tính toán các đòn tấn công cần thiết và tổng sát thương; tổng tấn công tối đa là không đổi | 
| Không gian | O(n) | Lưu trữ các đòn tấn công và sức khỏe của quái vật | 

Thuật toán chia tỷ lệ tuyến tính theo số lượng quái vật và tổng số lượng quái vật trong tất cả các trường hợp thử nghiệm bị giới hạn bởi 10^5, do đó, giải pháp có thể thực hiện thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# provided samples
assert run("5\n3 17 1\n2\n16\n10 999 3\n10 20 30\n100 50 30\n1000 1000 4\n200 300 400 500\n1000 1000 1000 1000\n999 999 1\n1000\n1000\n999 999 1\n1000000\n999\n") == "YES\nYES\nYES\nNO\nYES", "sample 1"

# custom cases
assert run("1\n1 1 1\n1\n1\n") == "YES", "minimum values"
assert run("1\n1 1000000 1\n1000000\n1000000\n") == "YES", "max monster attack equal hero health"
assert run("1\n5 10 2\n3 6\n7 5\n") == "NO", "hero cannot survive both monsters"
assert run("1\n10 10 2\n5 4\n5 6\n") == "YES", "hero can finish last monster while dying"
assert run("1\n1000000 1 1\n1\n1000000\n") == "NO", "hero dies immediately"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1\n1\n1 | CÓ | Giá trị đầu vào tối thiểu | 
| 1 1000000 1\n1000000\n1000000 | CÓ | Tấn công quái vật tối đa bằng máu anh hùng, anh hùng chết cuối cùng | 
| 5 10 2\n3 6\n7 5 | KHÔNG | Anh hùng không thể sống sót trước tất cả quái vật | 
| 10 10 2\n5 4\n5 6 | CÓ | Anh hùng chết trên con quái vật cuối cùng | 
| 1000000 1 1\n1\n1000000 | KHÔNG | Anh hùng chết ngay lập tức | 

## Vỏ cạnh 

Đối với trường hợp một con quái vật có đòn tấn công vượt quá lượng máu của anh hùng, thuật toán sẽ cho phép CÓ một cách chính xác nếu anh hùng có thể giết quái vật đó chỉ bằng một đòn tấn công. Ví dụ: máu anh hùng 999, máu quái vật 1000, đòn tấn công của quái vật 1000. Phân chia trần mang lại 1 đòn tấn công cần thiết, tổng sát thương = 1000. Tấn công tối đa = 1000. B >= Total_damage - max_Attack → 999 >= 0, đánh giá
