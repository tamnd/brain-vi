---
title: "CF 104992D - \u0421\u043a\u043e\u043b\u044c\u043a\u043e \u043e\u0448\u0438\u0431\u043e\u043a?"
description: "Quá trình đào tạo tạo ra một chuỗi các bài tập. Mỗi bài tập có một “câu trả lời đúng” do con cú đưa ra và câu trả lời do Grisha viết. Bài tập tương tự có thể xuất hiện nhiều lần, vì nếu câu trả lời của Grisha không được chấp nhận, con cú sẽ lặp lại bài tập đó sau."
date: "2026-06-28T04:27:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104992
codeforces_index: "D"
codeforces_contest_name: "qual VKOSHP Junior 24"
rating: 0
weight: 104992
solve_time_s: 61
verified: false
draft: false
---

[CF 104992D - \u0421\u043a\u043e\u043b\u044c\u043a\u043e \u043e\u0448\u0438\u0431\u043e\u043a?](https://codeforces.com/problemset/problem/104992/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Quá trình đào tạo tạo ra một chuỗi các bài tập. Mỗi bài tập có một “câu trả lời đúng” do con cú đưa ra và câu trả lời do Grisha viết. Bài tập tương tự có thể xuất hiện nhiều lần, vì nếu câu trả lời của Grisha không được chấp nhận, con cú sẽ lặp lại bài tập đó sau. 

Đối với mỗi lần xuất hiện, chúng ta được cung cấp hai chuỗi: câu trả lời của Grisha trước, sau đó là câu trả lời đúng. Mặc dù các chuỗi có thể khác nhau một chút về định dạng, quy tắc chấp nhận rất dễ chịu: sự khác biệt về kiểu chữ, dấu câu và thứ tự từ bị bỏ qua và các biến thể chính tả nhỏ được coi là không liên quan theo cùng một ý tưởng chuẩn hóa. Trong thực tế, điều này có nghĩa là hai câu trả lời sẽ được coi là tương đương nếu chúng bao gồm cùng một bộ từ sau khi loại bỏ dấu câu và sự khác biệt về kiểu chữ, đồng thời thứ tự từ không còn quan trọng nữa. 

Nhiệm vụ không phải là đếm số lần không khớp trong mỗi lần thử. Thay vào đó, chúng ta phải đếm xem Grisha đã thất bại bao nhiêu bài tập riêng biệt ngay lần đầu tiên chúng xuất hiện. Khi một bài tập được trả lời đúng lần đầu tiên, bất kỳ sự lặp lại nào sau đó của bài tập đó đều không liên quan đến câu trả lời. 

Quan sát quan trọng là việc nhận dạng một bài tập được xác định bởi câu trả lời đúng sau khi chuẩn hóa, bởi vì cùng một nhiệm vụ luôn đi kèm với cùng một chuỗi giải pháp đúng. Do đó, chúng tôi chỉ quan tâm đến lần đầu tiên mỗi câu trả lời đúng chuẩn hóa xuất hiện và liệu câu trả lời chuẩn hóa của Grisha có khớp với nó hay không. 

Các ràng buộc cho phép tối đa 40.000 lần thử và tổng kích thước đầu vào lên tới 200.000 ký tự. Điều này loại trừ bất kỳ giải pháp nào liên tục so sánh các chuỗi theo kiểu bậc hai. Việc xử lý trước tuyến tính hoặc gần tuyến tính cho mỗi chuỗi là đủ, nhưng việc sắp xếp lặp đi lặp lại các cấu trúc lớn mà không cẩn thận vẫn sẽ vượt qua do tổng chiều dài nhỏ, miễn là chúng ta tổng hợp chính xác. 

Một trường hợp khó phát hiện khi cùng một bài tập xuất hiện nhiều lần và lần đầu được trả lời sai, sau đó là trả lời đúng. Ví dụ: nếu bài tập “A” ban đầu xuất hiện là sai, sau đó xuất hiện lại, chúng ta vẫn chỉ phải tính nó một lần. Một trường hợp đặc biệt khác là hai chuỗi trông khác nhau về bề ngoài có thể thực sự biểu thị cùng một câu trả lời khi chuẩn hóa, do đó so sánh chuỗi trực tiếp sẽ tính sai số lỗi. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ xử lý từng cặp một cách độc lập và so sánh trực tiếp hai chuỗi theo quy tắc tương đương tùy chỉnh. Đối với mỗi cặp trong số n cặp, chúng ta sẽ chuẩn hóa cả hai chuỗi bằng cách viết thường, loại bỏ dấu câu, tách thành các từ, sắp xếp chúng và sau đó so sánh. Điều này đã được chấp nhận về mặt độ phức tạp vì tổng kích thước đầu vào nhỏ nhưng việc triển khai đơn giản có thể vô tình xử lý lại cùng một câu trả lời đúng nhiều lần hoặc cố gắng chuẩn hóa lặp lại theo cách lồng nhau, dẫn đến chi phí không cần thiết. 

Cái nhìn sâu sắc về cấu trúc quan trọng là mỗi bài tập được xác định duy nhất bằng câu trả lời đúng sau khi chuẩn hóa. Vì các lần lặp lại của cùng một tác vụ luôn sử dụng lại cùng một chuỗi chính xác nên chúng ta có thể nhóm các lần thử theo mã định danh đó. Chúng ta chỉ cần biết liệu lần xuất hiện đầu tiên của mỗi mã định danh có chính xác hay không. 

Điều này làm giảm vấn đề trong việc duy trì một từ điển được khóa bằng các câu trả lời đúng được chuẩn hóa. Đối với mỗi cặp mới, chúng tôi tính toán biểu diễn chuẩn của cả hai chuỗi và so sánh chúng. Nếu nhiệm vụ chưa được nhìn thấy trước đó, chúng tôi sẽ quyết định xem có tăng câu trả lời dựa trên tính chính xác hay không và sau đó đánh dấu nó là đã thấy. Những lần xuất hiện sau đó sẽ bị bỏ qua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 

|---|---|---| 

| Brute Force chỉ so sánh mỗi cặp | O(tổng chiều dài các từ đăng nhập) | O(1) thêm | Đã chấp nhận |

| Băm theo câu trả lời đúng được chuẩn hóa | O(tổng chiều dài các từ đăng nhập) | O(tổng số từ) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lần thử n, sau đó xử lý 2n dòng thành n cặp (câu trả lời của Grisha, câu trả lời đúng). Mỗi cặp tương ứng với một lần nộp bài. 
2. Đối với mỗi chuỗi, hãy chuyển đổi nó thành dạng chuẩn bằng cách trích xuất các từ, chuyển chúng thành chữ thường và sắp xếp danh sách các từ thu được. Sự chuyển đổi này loại bỏ sự nhạy cảm với dấu câu, viết hoa và trật tự từ. 
3. Xây dựng khóa cho câu trả lời đúng bằng cách sử dụng dạng chuẩn này. Chìa khóa này đại diện cho danh tính của bài tập. 
4. Duy trì một từ điển lưu trữ xem chúng ta đã xử lý bài tập này chưa và liệu lần xuất hiện đầu tiên của nó có chính xác hay không. 
5. Khi gặp một bài tập mới lần đầu tiên, hãy so sánh dạng chuẩn của câu trả lời của Grisha và câu trả lời đúng. Nếu chúng khác nhau, hãy tăng bộ đếm lỗi. 
6. Đánh dấu bài tập là đã xem để bỏ qua những lần lặp lại trong tương lai. 

Ý tưởng cốt lõi là lần xuất hiện đầu tiên là lần duy nhất có thể đóng góp vào câu trả lời. Bất kỳ sự lặp lại nào sau đó đều không thể ảnh hưởng đến số lượng vì bài tập đã được giải quyết. 

### Tại sao nó hoạt động 

Biểu diễn chuẩn sẽ thu gọn tất cả các câu trả lời tương đương thành các chuỗi giống hệt nhau. Vì tất cả các lần lặp lại của cùng một bài tập đều có cùng một câu trả lời đúng nên chúng có chung khóa chính tắc. Từ điển đảm bảo rằng chỉ lần xuất hiện đầu tiên của mỗi khóa mới được đánh giá, vì vậy mỗi bài tập đóng góp tối đa một quyết định vào số đếm cuối cùng. Điều này đảm bảo tính chính xác vì bài toán hỏi về tính đúng đắn trong lần thử đầu tiên cho mỗi bài tập riêng biệt chứ không phải cho mỗi lần xuất hiện. 

## Giải pháp Python```python
import sys
import re
input = sys.stdin.readline

def normalize(s: str):
    words = re.findall(r"[a-zA-Z]+", s.lower())
    words.sort()
    return " ".join(words)

n = int(input())
seen = {}
errors = 0

for _ in range(n):
    grisha = input().rstrip("\n")
    correct = input().rstrip("\n")

    key = normalize(correct)
    if key not in seen:
        if normalize(grisha) != key:
            errors += 1
        seen[key] = True

print(errors)
```Giải pháp này xây dựng một biểu diễn chuẩn cho cả câu trả lời của học sinh và câu trả lời đúng bằng cách sử dụng cùng một quy trình chuẩn hóa. Biểu thức chính quy trích xuất các từ trong khi bỏ qua các thành phần dấu câu và khoảng cách. Sắp xếp đảm bảo rằng sự khác biệt về thứ tự từ không ảnh hưởng đến sự bình đẳng. Từ điển theo dõi xem chúng ta đã xử lý một bài tập nhất định hay chưa, đảm bảo chỉ lần xuất hiện đầu tiên mới góp phần đưa ra câu trả lời. 

Một lỗi phổ biến là so sánh các chuỗi thô hoặc chỉ các phiên bản chữ thường, lỗi này không thành công khi thay đổi thứ tự từ hoặc dấu câu được chèn vào. Một nguyên nhân khác là quên loại bỏ các tác vụ trùng lặp, điều này sẽ khiến lỗi bị tính quá mức. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

Đầu vào bao gồm bốn lần thử tạo thành hai bài tập, trong đó một bài tập lặp lại sau khi thất bại. 

| Bước | Grisha | Đúng | Chuẩn hóa đúng | Lần đầu tiên nhìn thấy? | Cuộc thi đấu? | Lỗi | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | Ai đã đăng bức ảnh này | Ai đã đăng bức ảnh này | bức ảnh này đăng ai | vâng | không | 1 | 
| 2 | Không có gì | Không có gì | bạn có được chào đón không | vâng | vâng | 1 | 
| 3 | Ai đã đăng bức ảnh này | Ai đã đăng bức ảnh này | bức ảnh này đăng ai | không | bỏ qua | 1 | 
| 4 | Ai đã đăng bức ảnh này | Ai đã đăng bức ảnh này | bức ảnh này đăng ai | không | bỏ qua | 1 | 

Dấu vết này cho thấy rằng chỉ lần xuất hiện đầu tiên của bài tập đầu tiên mới quan trọng. Mặc dù những lần xuất hiện sau đó không chính xác hoặc được sửa chữa nhưng chúng không ảnh hưởng đến số đếm cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng chiều dài log k) | Mỗi chuỗi được mã hóa và sắp xếp theo từ và tổng số ký tự trên tất cả các chuỗi được giới hạn bởi 200k | 
| Không gian | O(tổng số từ) | Lưu trữ để chuẩn hóa và theo dõi các bài tập đã xem | 

Độ phức tạp vừa vặn trong giới hạn vì cả n và tổng kích thước đầu vào đều nhỏ và mỗi chuỗi được xử lý độc lập bằng các phép biến đổi nhẹ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    import re

    def norm(s):
        w = re.findall(r"[a-zA-Z]+", s.lower())
        w.sort()
        return " ".join(w)

    seen = {}
    ans = 0

    for _ in range(n):
        g = input().rstrip("\n")
        c = input().rstrip("\n")
        k = norm(c)
        if k not in seen:
```
