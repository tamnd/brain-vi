---
title: "CF 1265A - Dây Đẹp"
description: "Chúng ta có một số chuỗi độc lập, mỗi chuỗi bao gồm các ký tự a, b, c và ?. Nhiệm vụ là thay thế mọi ? bằng một trong các chữ cái a, b hoặc c sao cho chuỗi cuối cùng không có hai ký tự liền kề bằng nhau."
date: "2026-06-11T20:30:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1265
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 604 (Div. 2)"
rating: 1000
weight: 1265
solve_time_s: 85
verified: true
draft: false
---

[CF 1265A - Dây đẹp](https://codeforces.com/problemset/problem/1265/A) 

**Đánh giá:** 1000 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số chuỗi độc lập, mỗi chuỗi bao gồm các ký tự`a`,`b`,`c`, Và`?`. Nhiệm vụ là thay thế mọi`?`với một trong những chữ cái`a`,`b`, hoặc`c`sao cho chuỗi cuối cùng không có hai ký tự liền kề bằng nhau. Nếu không có cách nào để hoàn thành việc thay thế trong khi vẫn duy trì đặc tính này, chúng tôi phải báo cáo lỗi. 

Một cách hữu ích để suy nghĩ về vấn đề là mỗi`?`là một vị trí trống, nhưng chuỗi đã áp đặt các ràng buộc trong đó các chữ cái được cố định. Chuỗi cuối cùng phải là một chuỗi hợp lệ trên bảng chữ cái 3 chữ cái với hạn chế là các vị trí liên tiếp không thể khớp nhau. 

Các ràng buộc nhỏ trên mỗi trường hợp thử nghiệm, nhưng tổng độ dài trên tất cả các trường hợp thử nghiệm đạt tới 100000. Điều này ngay lập tức gợi ý chiến lược thời gian tuyến tính cho mỗi trường hợp thử nghiệm, vì bất kỳ điều gì tệ hơn O(n) trên mỗi chuỗi đều có nguy cơ vượt quá khoảng 10^8 thao tác trong tập hợp tồi tệ nhất, không an toàn trong giới hạn 1 giây trong Python. 

Một mối quan tâm ngây thơ là liệu những lựa chọn địa phương tham lam sau này có thể dẫn đến mâu thuẫn hay không. Một trường hợp tinh vi khác là khi các chữ cái cố định đã vi phạm quy tắc, chẳng hạn`"aa?b"`, trong đó không có sự thay thế nào có thể khắc phục được sự kề cận không hợp lệ ban đầu`"aa"`. Trường hợp đó phải sản xuất ngay`-1`. 

Ngoài ra còn có một kiểu lỗi ít rõ ràng hơn: một chuỗi như`"a??a"`không phải lúc nào cũng cho phép điền hợp lệ, vì các điểm cuối buộc vùng ở giữa thành một mẫu lặp lại có thể va chạm. Ví dụ`"a??a"`có chiều dài 4 buộc hai vị trí ở giữa để tránh cả hai`a`và lẫn nhau, điều này vẫn khả thi, nhưng các ràng buộc xen kẽ dài hơn có thể chặn tất cả các tùy chọn trong các cấu hình cố định nhất định. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xử lý từng`?`dưới dạng lựa chọn phân nhánh của ba khả năng và cố gắng điền vào chuỗi, kiểm tra tính hợp lệ sau đó. Nếu có k dấu chấm hỏi thì sẽ có 3^k kết hợp. Ngay cả với k khoảng 20, điều này đã trở nên không khả thi và ở đây tổng k có thể lên tới 10^5 qua các thử nghiệm. Sự bùng nổ xuất phát từ việc xử lý các lựa chọn một cách độc lập mà không sử dụng thực tế là các ràng buộc liền kề hoàn toàn mang tính cục bộ. 

Quan sát quan trọng là mỗi vị trí chỉ tương tác với các vị trí lân cận của nó. Khi xử lý chuỗi từ trái sang phải, khi chúng ta quyết định một ký tự ở vị trí i thì nó chỉ ảnh hưởng đến i+1. Điều này loại bỏ sự phụ thuộc toàn cầu và làm cho việc xây dựng tham lam trở nên khả thi. 

Thay vì phân nhánh, chúng tôi thực thi một quy tắc cục bộ: khi chúng tôi gặp phải một`?`, chúng tôi chọn bất kỳ chữ cái nào khác với hàng xóm bên trái đã được xác định của nó. Các vị trí sau này sẽ giải quyết tương tự và vì có ba chữ cái nên chúng ta luôn có ít nhất một lựa chọn hợp lệ miễn là chúng ta tránh vi phạm các ràng buộc cố định. 

Lần duy nhất chúng tôi thất bại là khi các chữ cái cố định đã tạo ra một vùng lân cận không hợp lệ hoặc khi một`?`bị buộc phải mâu thuẫn bởi cả hai người hàng xóm đều bình đẳng và cạn kiệt mọi lựa chọn. Nhưng với ba nhân vật, hai người hàng xóm bị cấm vẫn để lại một lựa chọn hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^k) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Trước tiên, chúng tôi kiểm tra xem dữ liệu đầu vào đã có vi phạm giữa các ký tự cố định hay chưa. Chúng tôi quét các cặp liền kề và nếu chúng tôi thấy hai ký tự giống nhau nhưng không`?`, chúng tôi đã biết câu trả lời là không thể. Điều này là cần thiết bởi vì`?`không thể khắc phục được mâu thuẫn đã có từ trước. 
2. Chúng tôi chuyển đổi chuỗi thành một mảng có thể thay đổi để dễ dàng thực hiện việc thay thế. 
3. Chúng tôi quét từ trái sang phải. Tại mỗi vị trí i, chúng ta quyết định nên đặt ký tự nào. 
4. Nếu nhân vật không`?`, chúng tôi giữ nguyên như vậy. Điều này bảo tồn các ràng buộc đã được thực thi bởi đầu vào. 
5. Nếu có`?`, chúng tôi cố gắng chỉ định một trong`a`,`b`, hoặc`c`sao cho nó khác với ký tự trước đó (nếu nó tồn tại) và cũng khác với ký tự tiếp theo nếu ký tự tiếp theo đó đã được sửa. Chúng tôi chọn ứng cử viên hợp lệ đầu tiên. 
6. Nếu không đặt được ký tự nào, chúng ta kết luận ngay rằng chuỗi không thể đẹp được. 
7. Sau khi điền, chúng tôi thực hiện bước xác thực cuối cùng để đảm bảo không tồn tại các ký tự bằng nhau liền kề và xuất kết quả. 

Tại sao điều này hoạt động xuất phát từ cấu trúc của các ràng buộc. Mỗi vị trí chỉ cấm tối đa hai chữ cái: chữ cái ở bên trái và chữ cái ở bên phải nếu đã cố định. Vì chúng ta có tổng cộng ba chữ cái nên luôn có ít nhất một lựa chọn trừ khi đã tồn tại mâu thuẫn ở các vị trí cố định. Quyết định tham lam là an toàn vì nó không bao giờ loại bỏ tất cả các lựa chọn trong tương lai đối với các vị trí nằm ngoài vị trí liền kề; mọi ràng buộc đều cục bộ và đối xứng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(s: str):
    n = len(s)
    s = list(s)

    # early check for fixed contradictions
    for i in range(n - 1):
        if s[i] != '?' and s[i] == s[i + 1]:
            return "-1"

    for i in range(n):
        if s[i] == '?':
            for c in "abc":
                ok = True
                if i > 0 and s[i - 1] == c:
                    ok = False
                if i + 1 < n and s[i + 1] != '?' and s[i + 1] == c:
                    ok = False
                if ok:
                    s[i] = c
                    break
            if s[i] == '?':
                return "-1"

    for i in range(n - 1):
        if s[i] == s[i + 1]:
            return "-1"

    return "".join(s)

def main():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        print(solve(s))

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng việc kiểm tra độ chính xác đối với các ký tự đã được cố định vì không có sự thay thế nào có thể sửa chữa một phần kề không hợp lệ đã tồn tại. Bước điền tham lam dựa vào việc thử các chữ cái theo một thứ tự cố định, nhưng bất kỳ thứ tự nào cũng hợp lệ vì tính khả thi không phụ thuộc vào sự lựa chọn tối ưu mà chỉ phụ thuộc vào sự tồn tại. 

Điều tinh tế quan trọng là chỉ kiểm tra hàng xóm phù hợp khi nó đã được biết đến. Nếu chúng tôi yêu cầu sự nhất quán trong tương lai mà không cho phép`?`, chúng ta sẽ hạn chế quá mức quyết định. Cách xử lý từ trái sang phải này đảm bảo chúng ta không bao giờ làm mất hiệu lực các lựa chọn trong quá khứ. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào đại diện. 

### Ví dụ 1:`"a???cb"`Chúng tôi theo dõi các quyết định từ trái sang phải. 

| tôi | char | hàng xóm bên trái | hàng xóm bên phải | đã chọn | 
| --- | --- | --- | --- | --- | 
| 0 | một | - | ? | một | 
| 1 | ? | một | ? | b | 
| 2 | ? | b | ? | một | 
| 3 | ? | một | c | b | 
| 4 | c | b | b | c | 
| 5 | b | c | - | b | 

Chuỗi cuối cùng trở thành`"ababcb"`. Điều này chứng tỏ việc lựa chọn tham lam luôn mang lại sự linh hoạt nhờ ba ký tự có sẵn. 

### Ví dụ 2:`"a??bbc"`Chúng tôi lại tiến hành tuần tự. 

| tôi | char | hàng xóm bên trái | hàng xóm bên phải | đã chọn | 
| --- | --- | --- | --- | --- | 
| 0 | một | - | ? | một | 
| 1 | ? | một | ? | b | 
| 2 | ? | b | b | một | 
| 3 | b | một | b | b | 
| 4 | b | b | c | xung đột | 
| 5 | c | b | - | c | 

Ở vị trí 4, chúng ta đã có`"bb"`từ đầu vào cố định, vi phạm quy tắc bất kể lựa chọn trước đó. Thuật toán phát hiện sự mâu thuẫn này và trả về`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một số lần không đổi, tối đa 3 lần kiểm tra ứng cử viên | 
| Không gian | O(n) | Chúng tôi lưu trữ một biểu diễn mảng có thể thay đổi của chuỗi | 

Tổng chiều dài của các trường hợp thử nghiệm là 100000, do đó việc xử lý tuyến tính nằm trong giới hạn thoải mái. Mỗi trường hợp thử nghiệm là độc lập và không có vòng lặp lồng nhau nào phụ thuộc vào độ dài chuỗi vượt quá các yếu tố không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve(s: str):
        n = len(s)
        s = list(s)

        for i in range(n - 1):
            if s[i] != '?' and s[i] == s[i + 1]:
                return "-1"

        for i in range(n):
            if s[i] == '?':
                for c in "abc":
                    ok = True
                    if i > 0 and s[i - 1] == c:
                        ok = False
                    if i + 1 < n and s[i + 1] != '?' and s[i + 1] == c:
                        ok = False
                    if ok:
                        s[i] = c
                        break
                if s[i] == '?':
                    return "-1"

        for i in range(n - 1):
            if s[i] == s[i + 1]:
                return "-1"

        return "".join(s)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve(input().strip()))
    return "\n".join(out)

# provided samples
assert run("3\na???cb\na??bbc\na?b?c") == "ababcb\n-1\nacbac"

# custom cases
assert run("1\na") == "a"
assert run("1\na?") == "ab"
assert run("1\naa?") == "-1"
assert run("1\na????a") != "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"a"`|`"a"`| chuỗi hợp lệ tối thiểu | 
|`"a?"`|`"ab"`| thay thế duy nhất với ràng buộc ranh giới | 
|`"aa?"`|`-1`| mâu thuẫn cố định ngay lập tức | 
|`"a????a"`| chuỗi hợp lệ | lan truyền ràng buộc dài | 

## Vỏ cạnh 

Trường hợp lỗi trực tiếp là khi các ký tự cố định đã vi phạm quy tắc kề. Đối với đầu vào`"aa?b"`, quá trình quét phát hiện`"aa"`lúc bắt đầu và trả về`-1`ngay lập tức. Không có sự thay thế nào có thể thay đổi các ký tự đã cố định, do đó thuật toán sẽ loại bỏ một cách chính xác mà không cần cố gắng xây dựng. 

Một trường hợp tế nhị khác là`"a????a"`. Các điểm cuối buộc nội thất phải tránh`a`và tránh sự lặp lại nội bộ. Thủ tục tham lam thực hiện từ trái sang phải, luôn có ít nhất một trong các`{a, b, c}`có sẵn để tránh ký tự trước đó và cuối cùng tạo ra mẫu xen kẽ hợp lệ như`"ababca"`hoặc`"acbac a"`tùy vào sự lựa chọn. Tính bất biến chỉ có một lân cận quan trọng tại thời điểm gán đảm bảo rằng các điểm cuối không gây ra mâu thuẫn toàn cục ngoài các ràng buộc kề cận cục bộ.
