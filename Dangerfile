# Greet Devs!
welcome_message.greet

# Make sure if PR have assignee
warn "This PR does not have any assignees yet." unless github.pr_json["assignee"]

# Make it more obvious that a PR is a work in progress and shouldn't be merged yet
warn "PR is classed as Work in Progress" if github.pr_labels.include? "WIP"
warn "PR is classed as Work in Progress" if github.pr_title.include? "[WIP]"

# Provide PR Summary
if github.pr_body.length < 5
    fail "Please provide a summary in the Pull Request description"
end

# has_milestone = github.pr_json["milestone"] != nil
# fail("This PR does not refer to an existing milestone", sticky: false) unless has_milestone

# github.pr_diff.split(/\n/).grep(/^\+/).each do |added_line|
#   contains_request_manager = added_line.match(/BLRequestOperationManager.requestManager.post/) || 
#     added_line.match(/BLRequestOperationManager.requestManager.get/) || 
#     added_line.match(/BLRequestOperationManager.requestManager.delete/) || 
#     added_line.match(/BLRequestOperationManager.requestManager.patch/)

#   contains_request = added_line.match(/request\(\).post/) || 
#     added_line.match(/request\(\).get/) || 
#     added_line.match(/request\(\).delete/) || 
#     added_line.match(/request\(\).patch/)

#   contains_service_v2 = added_line.match(/ServiceV2/)

#   if contains_service_v2 || contains_request_manager || contains_request
#     fail "Please don't add more APIV2 call in line `#{added_line}`"
#   end
# end