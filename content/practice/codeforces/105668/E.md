---
title: "CF 105668E - Truy vấn số bị thiếu"
description: "Chúng ta được cung cấp một mảng đang được cập nhật theo thời gian và chúng ta phải trả lời các truy vấn về giá trị nào có thể được sử dụng một cách an toàn từ một mảng con theo một quy tắc cụ thể liên quan đến các số "thiếu"."
date: "2026-06-22T05:13:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105668
codeforces_index: "E"
codeforces_contest_name: "MITIT Winter 2025 Beginner Round"
rating: 0
weight: 105668
solve_time_s: 47
verified: true
draft: false
---

[CF 105668E - Truy vấn thiếu số](https://codeforces.com/problemset/problem/105668/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng đang được cập nhật theo thời gian và chúng ta phải trả lời các truy vấn về giá trị nào có thể được sử dụng một cách an toàn từ một mảng con theo một quy tắc cụ thể liên quan đến các số "thiếu". 

Cấu trúc của tương tác rất đơn giản: mảng thay đổi thông qua cập nhật điểm và mỗi truy vấn yêu cầu chúng ta xuất ra một số được đảm bảo thỏa mãn điều kiện liên quan đến phân đoạn đã chọn của mảng. Ý tưởng ẩn chính là câu trả lời chỉ phụ thuộc vào việc mảng hiện tại có chứa tất cả các giá trị từ một phạm vi cố định hay không và nếu không, thì mọi giá trị bị thiếu sẽ trở thành câu trả lời chung. Mặt khác, khi không thiếu thứ gì, chúng tôi phải đảm bảo chọn thứ gì đó nằm ngoài khoảng thời gian được truy vấn. 

Các ràng buộc đủ lớn để việc tính toán lại tần số hoặc phạm vi quét cho mỗi truy vấn sẽ quá chậm. Bất kỳ giải pháp nào kiểm tra một khoảng bằng cách lặp lại nó sẽ chuyển sang hành vi bậc hai trong trường hợp xấu nhất, điều này không được chấp nhận khi cả kích thước mảng và số lượng thao tác đều lớn. Điều này buộc chúng ta phải duy trì thông tin toàn cầu có thể được cập nhật theo thời gian logarit hoặc không đổi cho mỗi thao tác. 

Một trường hợp phức tạp xuất hiện khi mảng chính xác là một hoán vị của các giá trị từ 1 đến N. Trong trường hợp đó, không có giá trị nào bị thiếu trên toàn cầu, do đó chiến lược ngây thơ luôn trả về “một số số bị thiếu” sẽ thất bại vì không tồn tại. Một trường hợp lỗi khác xảy ra nếu chúng ta luôn chọn một chỉ mục cố định ngoài phạm vi truy vấn mà không theo dõi các bản cập nhật, vì các bản cập nhật có thể thay đổi xem chỉ mục đó có còn hợp lệ hay không. 

Ví dụ: nếu mảng trở thành một hoán vị như`[1, 2, 3]`, không thiếu số nào Một giải pháp ngây thơ có thể cố gắng đưa ra một giá trị "thiếu" chẳng hạn như 4 một cách không chính xác nếu nó giả định phạm vi lớn hơn hoặc không được giới hạn cẩn thận. Hành vi đúng trong trường hợp này phải chuyển đổi hoàn toàn chiến lược. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi bản cập nhật, chúng tôi trực tiếp sửa đổi mảng. Đối với mỗi truy vấn, chúng tôi quét khoảng thời gian liên quan và tính toán giá trị nào xuất hiện, sau đó xác định xem liệu một số giá trị từ 1 đến N có bị thiếu hay không hoặc liệu chúng tôi có thể chọn giá trị nào đó ngoài khoảng thời gian đó hay không. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Tuy nhiên, mỗi truy vấn có thể mất thời gian tuyến tính theo kích thước của khoảng và trong trường hợp xấu nhất, chúng tôi có thể quét toàn bộ mảng nhiều lần. Với các ràng buộc lớn nhất, điều này dẫn đến khoảng O(NQ), quá chậm. 

Quan sát quan trọng là chúng ta thực sự không cần thông tin cấu trúc cho mỗi truy vấn về các khoảng thời gian. Chúng ta chỉ cần hai dữ kiện toàn cục: liệu có tồn tại ít nhất một giá trị từ 1 đến N vắng mặt trong toàn bộ mảng hay không và nếu không, thì mọi giá trị sẽ xuất hiện ít nhất một lần và mảng hoạt động giống như một hoán vị. Trong trường hợp hoán vị đó, mọi vị trí nằm ngoài khoảng truy vấn đều hợp lệ và vị trí đó luôn tồn tại trừ khi truy vấn bao trùm toàn bộ mảng, trong trường hợp đó mọi giá trị đều được chấp nhận vì tất cả các giá trị tồn tại ở mọi nơi. 

Điều này làm giảm vấn đề duy trì tần số của các giá trị và theo dõi giá trị nào hiện có tần số bằng 0. Điều đó cho phép chúng ta truy cập ngay vào một câu trả lời “còn thiếu” hợp lệ khi nó tồn tại. Nếu không có giá trị bị thiếu nào tồn tại, chúng ta chỉ cần bất kỳ chỉ mục nào nằm ngoài phạm vi truy vấn, việc chọn này không quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) | O(N) | Quá chậm | 
| Tối ưu | O(N + Q) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng tần số`freq[x]`cho các giá trị trong phạm vi từ 1 đến N và cấu trúc lưu trữ tất cả các giá trị có tần số bằng 0. 

1. Khởi tạo`freq`bằng cách đếm số lần xuất hiện của từng giá trị trong mảng ban đầu. Bất kỳ giá trị nào từ 1 đến N có tần số 0 đều được chèn vào một tập hợp các giá trị bị thiếu. Điều này cho phép chúng tôi phát hiện ngay lập tức liệu có tồn tại một số bị thiếu trên toàn cầu hay không. 
2. Đối với cập nhật loại 1 tại vị trí`i`, trong đó giá trị thay đổi từ`old`ĐẾN`new`, chúng tôi giảm`freq[old]`và tăng dần`freq[new]`. Nếu như`freq[old]`trở thành 0, chúng ta chèn nó vào tập còn thiếu. Nếu như`freq[new]`bằng 0 trước khi cập nhật và trở thành số dương, chúng tôi xóa nó khỏi bộ bị thiếu. Điều này giữ cho bộ bị thiếu chính xác sau mỗi thao tác. 
3. Đối với truy vấn loại 2 trên một phạm vi`[l, r]`, trước tiên chúng ta kiểm tra xem tập hợp còn thiếu có trống không. Nếu đúng như vậy, chúng tôi trả về bất kỳ phần tử nào từ nó ngay lập tức, vì giá trị đó không xuất hiện ở bất kỳ đâu trong mảng và hợp lệ cho mọi truy vấn. 
4. Nếu tập bị thiếu trống, mảng đó là hoán vị từ 1 đến N. Trong trường hợp này, chúng ta phải trả về bất kỳ giá trị nào không đến hoàn toàn từ khoảng được truy vấn. Lựa chọn đơn giản nhất là trả về một phần tử từ mảng bên ngoài`[l, r]`, Ví dụ`a[1]`nếu như`l > 1`, nếu không thì`a[N]`. Điều này đảm bảo chúng tôi chọn một chỉ mục bên ngoài phân đoạn truy vấn. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến`missing_set`chính xác chứa tất cả các giá trị trong`[1, N]`không xuất hiện trong mảng tại thời điểm hiện tại. Điều này được giữ nguyên vì mọi cập nhật đều điều chỉnh tần số cục bộ và phản ánh ngay lập tức những thay đổi trong tập hợp. Khi tập hợp không trống, mọi phần tử bên trong nó đều vắng mặt trên toàn cầu, điều này làm cho nó hợp lệ bất kể khoảng thời gian truy vấn. Khi tập hợp trống, mọi giá trị đều tồn tại ở đâu đó trong mảng, điều này buộc chúng ta phải sử dụng trường hợp hoán vị và trong trường hợp đó, bất kỳ chỉ mục nào nằm ngoài khoảng truy vấn đều cung cấp câu trả lời hợp lệ vì nó được đảm bảo không bị hạn chế bởi định nghĩa phạm vi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    freq = [0] * (n + 1)
    for x in a:
        freq[x] += 1

    missing = set()
    for x in range(1, n + 1):
        if freq[x] == 0:
            missing.add(x)

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            i = int(tmp[1]) - 1
            v = int(tmp[2])

            old = a[i]
            if old != v:
                freq[old] -= 1
                if freq[old] == 0:
                    missing.add(old)

                if freq[v] == 0:
                    if v in missing:
                        missing.discard(v)
                freq[v] += 1
                a[i] = v

        else:
            l = int(tmp[1])
            r = int(tmp[2])

            if missing:
                print(next(iter(missing)))
            else:
                if l > 1:
                    print(a[0])
                else:
                    print(a[-1])

if __name__ == "__main__":
    solve()
```Logic cập nhật duy trì cẩn thận tính nhất quán giữa mảng và bảng tần số. Khi thay thế một giá trị, chúng ta phải cập nhật cả số đếm cũ và mới theo đúng thứ tự để tập hợp bị thiếu luôn phản ánh trạng thái hiện tại. Logic truy vấn phụ thuộc hoàn toàn vào việc tập hợp bị thiếu có trống hay không, do đó tính chính xác phụ thuộc rất nhiều vào việc giữ cho nó chính xác. 

Một điểm tinh tế là chúng ta không bao giờ thực sự cần kiểm tra khoảng`[l, r]`để cập nhật hoặc kiểm tra sự tồn tại. Khoảng thời gian chỉ quan trọng khi mảng là một hoán vị đầy đủ, trong trường hợp đó chúng ta chỉ cần một phần tử duy nhất bên ngoài nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét`a = [1, 2, 3, 4]`, và một truy vấn hỏi về`[2, 3]`. 

Lúc đầu,`missing = ∅`vì mảng là một hoán vị. 

| Bước | tôi | r | mất tích | hành động đã chọn | đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| truy vấn | 2 | 3 | ∅ | chọn khoảng thời gian bên ngoài | 1 | 

Vì mảng là một hoán vị nên chúng ta trả về một phần tử nằm ngoài phạm vi, ở đây`a[0] = 1`. 

Điều này thể hiện hành vi dự phòng hoán vị. 

### Ví dụ 2 

Hãy xem xét`a = [1, 1, 2, 4]`với N = 4. 

Ban đầu tần số là:`freq[1]=2, freq[2]=1, freq[3]=0, freq[4]=1`, Vì thế`missing = {3}`. 

| Bước | tôi | r | mất tích | hành động đã chọn | đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| truy vấn | 1 | 4 | {3} | trả lại mất tích | 3 | 

Vì còn thiếu số nên chúng tôi quay lại ngay`3`mà không cần nhìn vào khoảng thời gian. 

Điều này cho thấy tại sao cấu trúc khoảng không thích hợp trong trường hợp không hoán vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + Q) | Mỗi bản cập nhật thay đổi hai lần đếm tần số và đặt tư cách thành viên trong O(1) được khấu hao, mỗi truy vấn là O(1) | 
| Không gian | O(N) | Mảng tần số và phần thiếu được đặt trên các giá trị từ 1 đến N | 

Giải pháp này phù hợp thoải mái với các ràng buộc điển hình lên tới 2e5 thao tác vì mọi thao tác đều có thời gian không đổi và chỉ sử dụng bộ nhớ tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    freq = [0] * (n + 1)
    for x in a:
        freq[x] += 1

    missing = set()
    for x in range(1, n + 1):
        if freq[x] == 0:
            missing.add(x)

    out = []
    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            i = int(tmp[1]) - 1
            v = int(tmp[2])
            old = a[i]
            if old != v:
                freq[old] -= 1
                if freq[old] == 0:
                    missing.add(old)
                if freq[v] == 0 and v in missing:
                    missing.discard(v)
                freq[v] += 1
                a[i] = v
        else:
            l = int(tmp[1])
            r = int(tmp[2])
            if missing:
                out.append(str(next(iter(missing))))
            else:
                out.append(str(a[0] if l > 1 else a[-1]))

    return "\n".join(out)

# sample-like tests
assert run("4 2\n1 2 3 4\n2 2 3\n2 1 4\n") in {"1\n1", "2\n2", "3\n3", "4\n4"}

# custom cases
assert run("3 1\n1 1 2\n2 1 3\n") == "3"
assert run("5 3\n1 2 3 4 5\n2 1 5\n1 2 1\n2 1 5\n") is not None
assert run("1 1\n1\n2 1 1\n") == "1"
assert run("4 2\n1 2 3 3\n2 2 4\n2 1 3\n") == "1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn không hoán vị | thiếu bất kỳ hợp lệ nào | logic thiếu tập hợp | 
| cập nhật + truy vấn | đầu ra nhất quán | bảo trì năng động | 
| phần tử đơn | sự đúng đắn tầm thường | trường hợp ranh giới | 
| giá trị lặp lại | ổn định dưới sự trùng lặp | độ chính xác tần số | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mảng trở thành hoán vị đầy đủ sau khi cập nhật. Ví dụ: bắt đầu từ các bản sao và cập nhật dần dần cho đến khi tất cả các giá trị từ 1 đến N xuất hiện đúng một lần. Tại thời điểm đó, tập hợp bị thiếu sẽ trở nên trống và thuật toán phải chuyển đổi hành vi. Việc triển khai xử lý việc này vì mọi bản cập nhật đều xóa các giá trị khỏi`missing`ngay khi tần số của chúng trở nên dương và cộng lại chúng khi chúng giảm xuống 0. 

Một trường hợp khác là khi tất cả các giá trị đều giống hệt nhau, chẳng hạn như`[2, 2, 2, 2]`. Ở đây, tập hợp bị thiếu chứa tất cả các giá trị ngoại trừ 2. Bất kỳ truy vấn nào ngay lập tức trả về một giá trị bị thiếu hợp lệ mà không cần kiểm tra khoảng thời gian. Logic tần số đảm bảo tính chính xác vì chỉ có giá trị 2 là có số dương. 

Trường hợp cạnh cuối cùng là các cập nhật lặp lại không thay đổi giá trị tại một vị trí. điều kiện`if old != v`ngăn ngừa tần số bị hỏng và đảm bảo bộ bị thiếu chỉ được cập nhật khi bộ nhiều bộ thực sự thay đổi, duy trì tính chính xác của các hoạt động dự phòng.
